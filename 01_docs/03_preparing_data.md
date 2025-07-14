# 📕 Data Preparation Documentation

**📌 Overview**
This document outlines the data preparation process for travel booking platform analysis, focusing on creating a comprehensive session-level dataset enriched with user demographics, flight details, and hotel information.

## 📕 Data Preprocessing Pipeline

### 📌 1. Temporal Data Filtering
```sql
sessions_after_jan_5_2023 AS (
    SELECT *
    FROM sessions
    WHERE session_start >= '2023-01-05'
)
```
- **Objective**: Filter sessions to include only data from January 5, 2023 onwards
- **Rationale**: Focus analysis on recent user behavior patterns
- **Output**: Subset of sessions table with temporal constraints

### 📌 2. User Activity Filtering
```sql
users_with_more_than_7_sessions AS (
    SELECT user_id, COUNT(*) AS session_count
    FROM sessions_after_jan_5_2023
    GROUP BY user_id
    HAVING COUNT(*) > 7
)
```
- **Objective**: Identify high-engagement users for analysis
- **Criteria**: Users with more than 7 sessions
- **Output**: List of active user_ids with session counts

### 📌 3. Hotel Data Cleaning
```sql
ordered_check_in_out AS (
    SELECT
        trip_id,
        CASE WHEN check_out_time < check_in_time 
             THEN check_out_time ELSE check_in_time END AS cleaned_check_in_time,
        CASE WHEN check_out_time < check_in_time 
             THEN check_in_time ELSE check_out_time END AS cleaned_check_out_time
    FROM hotels
)
```
- **Objective**: Correct reversed check-in/check-out timestamps
- **Logic**: Swap timestamps when check_out_time precedes check_in_time
- **Output**: Standardized hotel booking timestamps

### 📌 4. Stay Duration Calculation
```sql
calculated_stay AS (
    SELECT
        trip_id,
        (cleaned_check_out_time::date - cleaned_check_in_time::date) AS cleaned_nights,
        CASE WHEN cleaned_check_out_time - cleaned_check_in_time < INTERVAL '1 day'
             THEN EXTRACT(EPOCH FROM (cleaned_check_out_time - cleaned_check_in_time)) / 3600.0
             ELSE NULL END AS duration_hours
    FROM ordered_check_in_out
)
```
- **Objective**: Calculate hotel stay duration in nights and hours
- **Logic**: 
  - Night calculation: Date difference between check-out and check-in
  - Hour calculation: For same-day stays (< 24 hours)
- **Output**: Stay duration metrics for each trip

### 📌 5. Comprehensive Data Enrichment
```sql
enriched_sessions_with_user_trip_data AS (
    SELECT
        -- Session identifiers
        s.session_id, s.user_id, s.trip_id,
        s.session_start, s.session_end, s.page_clicks,
        
        -- Binary conversion of booking flags
        s.flight_booked,
        CASE WHEN flight_booked = 'true' THEN 1 ELSE 0 END AS binary_flight_booked,
        
        -- User demographic data with age calculation
        u.birthdate,
        DATE_PART('year', AGE(CURRENT_DATE, u.birthdate)) AS customer_age,
        
        -- Geographic distance calculation
        COALESCE(haversine_distance(home_airport_lat, home_airport_lon,
                 destination_airport_lat, destination_airport_lon), 0) AS flown_flight_distance,
        
        -- Hotel name parsing
        LEFT(h.hotel_name, LENGTH(h.hotel_name) - POSITION(' - ' IN REVERSE(h.hotel_name)) - 2) AS extract_hotel_name,
        RIGHT(h.hotel_name, POSITION(' - ' IN REVERSE(h.hotel_name)) - 1) AS extract_hotel_location,
        
        -- Trip cancellation flag (window function)
        MAX(CASE WHEN cancellation = 'true' THEN 1 ELSE 0 END) OVER (PARTITION BY s.trip_id) AS trip_cancelled
        
    FROM sessions_after_jan_5_2023 s
    LEFT JOIN users u ON s.user_id = u.user_id
    LEFT JOIN flights f ON s.trip_id = f.trip_id
    LEFT JOIN hotels h ON s.trip_id = h.trip_id
    LEFT JOIN ordered_check_in_out oco ON s.trip_id = oco.trip_id
    LEFT JOIN calculated_stay cs ON s.trip_id = cs.trip_id
    WHERE s.user_id IN (SELECT user_id FROM users_with_more_than_7_sessions)
)
```

#### 📌 Key Transformations:
- **Boolean Conversion**: String 'true'/'false' values converted to binary 1/0
- **Age Calculation**: Dynamic age computation from birthdate
- **Distance Calculation**: Haversine formula for flight distance
- **String Parsing**: Hotel name and location extraction
- **Window Functions**: Trip-level cancellation status

## 📕 Data Quality Features

### Null Handling
- `COALESCE()` functions ensure zero values for missing numeric data
- `NULLIF()` prevents division by zero errors
- Default values assigned for critical metrics

### Data Validation
- Temporal consistency checks for hotel bookings
- Geographic coordinate validation
- Binary flag standardization

### Join Strategy
- **LEFT JOIN**: Preserves all session records
- **Incremental Enrichment**: Sequential data enhancement through CTEs
- **Foreign Key Relationships**: Maintained across users, flights, hotels tables

## 📕 Output Schema

### Session Level Metrics
- `session_id`, `user_id`, `trip_id`
- `session_start`, `session_end`, `page_clicks`

### User Demographics
- `customer_age`, `gender`, `married`, `has_children`
- `home_country`, `home_city`, `home_airport`

### Booking Behavior
- `binary_flight_booked`, `binary_hotel_booked`
- `binary_flight_discount`, `binary_hotel_discount`
- `binary_cancellation`, `trip_cancelled`

### Trip Details
- `flown_flight_distance`, `base_fare_usd`
- `cleaned_nights`, `hotel_price_per_room_night_usd`
- `seats`, `rooms`, `checked_bags`

### Calculated Fields
- `extract_hotel_name`, `extract_hotel_location`
- `cleaned_check_in_time`, `cleaned_check_out_time`

## 📕 Export Configuration
```python
df = pd.read_sql(query, engine)
export = pd.read_sql(query, engine)
```
- **Output**: Pandas DataFrame ready for downstream analysis
- **Format**: Session-level enriched dataset
- **Usage**: Foundation for user segmentation and modeling
