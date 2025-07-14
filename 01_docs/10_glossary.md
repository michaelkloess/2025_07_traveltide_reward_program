# 📕 Comprehensive Glossary

**📌 Overview**
This glossary provides complete definitions of all metrics, terms, and concepts used throughout the customer segmentation project documentation.

## 📕 Database Schema

### 📌 Table Structures

#### **users** - User demographic information
| Column | Data Type | Description |
|--------|-----------|-------------|
| `user_id` | int (key) | Unique user ID |
| `birthdate` | datetime | User date of birth |
| `gender` | nominal | User gender |
| `married` | binary | User marriage status |
| `has_children` | binary | Whether or not the user has children |
| `home_country` | nominal | User's resident country |
| `home_city` | nominal | User's resident city |
| `home_airport` | nominal | User's preferred hometown airport |
| `home_airport_lat` | decimal | Geographical north-south position of home airport |
| `home_airport_lon` | decimal | Geographical east-west position of home airport |
| `sign_up_date` | datetime | Date of TravelTide account creation |

#### **sessions** - Individual browsing sessions (sessions with ≥2 clicks only)
| Column | Data Type | Description |
|--------|-----------|-------------|
| `session_id` | string (key) | Unique browsing session ID |
| `user_id` | int (foreign key) | The user ID |
| `trip_id` | string (foreign key) | ID mapped to flight and hotel bookings |
| `session_start` | timestamp | Time of browsing session start |
| `session_end` | timestamp | Time of browsing session end |
| `flight_discount` | binary | Whether or not flight discount was offered |
| `hotel_discount` | binary | Whether or not hotel discount was offered |
| `flight_discount_amount` | decimal | Percentage off base fare |
| `hotel_discount_amount` | decimal | Percentage off base night rate |
| `flight_booked` | binary | Whether or not flight was booked |
| `hotel_booked` | binary | Whether or not hotel was booked |
| `page_clicks` | int | Number of page clicks during browsing session |
| `cancellation` | binary | Whether or not the purpose of the session was to cancel a trip |

#### **flights** - Information about purchased flights
| Column | Data Type | Description |
|--------|-----------|-------------|
| `trip_id` | string (key) | Unique trip ID |
| `origin_airport` | nominal | User's home airport |
| `destination` | nominal | Destination city |
| `destination_airport` | nominal | Airport in destination city |
| `seats` | int | Number of seats booked |
| `return_flight_booked` | binary | Whether or not a return flight was booked |
| `departure_time` | timestamp | Time of departure from origin airport |
| `return_time` | timestamp | Time of return to origin airport |
| `checked_bags` | int | Number of checked bags |
| `trip_airline` | nominal | Airline taking user from origin to destination |
| `destination_airport_lat` | decimal | Geographical north-south position of destination airport |
| `destination_airport_lon` | decimal | Geographical east-west position of destination airport |
| `base_fare_usd` | decimal | Pre-discount price of airfare |

#### **hotels** - Information about purchased hotel stays
| Column | Data Type | Description |
|--------|-----------|-------------|
| `trip_id` | string (key) | Unique trip ID |
| `hotel_name` | nominal | Hotel brand name |
| `nights` | int | Number of nights stayed at the hotel |
| `rooms` | int | Number of rooms booked with hotel |
| `check_in_time` | timestamp | Time user hotel stay begins |
| `check_out_time` | timestamp | Time user hotel stay ends |
| `hotel_per_room_usd` | decimal | Pre-discount price of hotel stay per room per night |

## 📕 Feature Engineering Metrics

### 📌 01_engagement_metrics
**Purpose:** Measure user interaction depth and platform usage patterns

| Metric | SQL Calculation | Definition |
|--------|----------------|------------|
| `session_frequency` | `COUNT(*)` | Total number of recorded user sessions within the analysis timeframe |
| `first_seen_date` | `MIN(session_start::DATE)` | Earliest known interaction date for a given user |
| `last_seen_date` | `MAX(session_end::DATE)` | Most recent session timestamp found in the dataset |
| `cancelled_session_count` | `SUM(binary_cancellation)` | Number of user actions flagged as canceled |
| `distinct_trip_count` | `COUNT(DISTINCT trip_id)` | Count of unique travel instances associated with the user |
| `active_presence_days` | `COUNT(DISTINCT DATE(session_start))` | Number of unique calendar days the user engaged with the platform |
| `mean_clicks_per_session` | `AVG(page_clicks)` | Average number of page interactions per recorded session |
| `average_session_time` | `AVG(EXTRACT(EPOCH FROM (session_end - session_start)))` | Mean length of time spent per session by a user (in seconds) |

### 📌 02_booking_behavior_metrics
**Purpose:** Quantify spending patterns and financial value generation

| Metric | SQL Calculation | Definition |
|--------|----------------|------------|
| `completed_flight_orders` | `SUM(CASE WHEN binary_flight_booked = 1 AND trip_cancelled = 0 THEN 1 ELSE 0 END)` | Total count of non-canceled flight transactions |
| `completed_hotel_orders` | `SUM(CASE WHEN binary_hotel_booked = 1 AND trip_cancelled = 0 THEN 1 ELSE 0 END)` | Number of successful hotel bookings excluding any reversals |
| `aggregate_flight_spend` | `SUM(COALESCE(base_fare_usd, 0))` | Sum of all flight-related expenditures by the user |
| `aggregate_hotel_spend` | `SUM(COALESCE(hotel_price_per_room_night_usd, 0) * COALESCE(rooms, 0) * COALESCE(cleaned_nights, 0))` | Total monetary value spent on hotel accommodations |
| `overall_transaction_value` | `aggregate_flight_spend + aggregate_hotel_spend` | Combined spending across both flight and hotel bookings |
| `trip_revenue_avg` | `SUM(base_fare_usd + hotel_price_total) / COUNT(DISTINCT trip_id)` | Average income per trip derived from completed bookings |
| `session_revenue_avg` | `SUM(base_fare_usd + hotel_price_total) / COUNT(session_id)` | Mean revenue calculated per session based on related purchases |
| `user_activity_lifespan` | `MAX(session_end::DATE) - MIN(session_start::DATE)` | Time span between a user's first interaction and their latest known activity |
| `mean_savings_value` | `SUM(flight_savings + hotel_savings)` | Average cost reduction achieved across bookings |
| `savings_per_kilometer` | `SUM(flight_savings) / SUM(flown_flight_distance)` | Typical amount saved per kilometer of flight distance |

### 📌 03_trip_dynamics_metrics
**Purpose:** Capture travel preferences and behavioral characteristics

| Metric | SQL Calculation | Definition |
|--------|----------------|------------|
| `conversion_ratio` | `SUM(CASE WHEN binary_flight_booked = 1 OR binary_hotel_booked = 1 THEN 1 ELSE 0 END) / COUNT(session_id)` | Proportion of sessions that resulted in a booking |
| `avg_nights_booked` | `SUM(cleaned_nights) / COUNT(DISTINCT trip_id)` | Mean number of hotel nights reserved per trip |
| `mean_room_quantity` | `SUM(rooms) / COUNT(DISTINCT trip_id)` | Average number of rooms included in hotel bookings |
| `mean_seat_count` | `SUM(seats) / COUNT(DISTINCT trip_id)` | Typical number of flight seats reserved per booking |
| `avg_flight_distance` | `SUM(flown_flight_distance) / COUNT(DISTINCT trip_id)` | Mean travel distance for flights, computed using geospatial methods |
| `avg_trip_timespan` | `AVG(return_time::DATE - departure_time::DATE)` | Typical duration of a trip from departure to return |
| `mean_checked_bags` | `SUM(checked_bags) / COUNT(DISTINCT trip_id)` | Average number of baggage units checked in per trip |
| `booking_lead_interval` | `AVG(departure_time::DATE - session_end::DATE)` | Average time between booking confirmation and the travel start date |
| `cancel_lead_interval` | `AVG(CASE WHEN cancellation = 'true' THEN (departure_time::DATE - session_end::DATE) ELSE NULL END)` | Typical time lag between a booking date and its cancellation |
| `hotel_spend_after_discount` | `SUM(hotel_price_per_room_night_usd * hotel_discount_amount * rooms * cleaned_nights)` | Total cost incurred on hotels after discounts were applied |
| `flight_spend_after_discount` | `SUM(base_fare_usd * flight_discount_amount * seats)` | Aggregated flight charges post-discount |
| `promo_booking_ratio` | `SUM(CASE WHEN binary_flight_discount = 1 OR binary_hotel_discount = 1 THEN 1 ELSE 0 END) / COUNT(session_id)` | Share of bookings made when a promotion or discount was active |
| `checked_bag_usage_ratio` | `SUM(checked_bags) / SUM(CASE WHEN binary_flight_booked = 1 THEN 1 ELSE 0 END)` | Rate at which users opted to check in luggage during air travel |
| `flight_hotel_mix_ratio` | `SUM(CASE WHEN binary_flight_booked = 1 THEN 1 ELSE 0 END) / SUM(CASE WHEN binary_hotel_booked = 1 THEN 1 ELSE 0 END)` | Proportional comparison between flight and hotel bookings per user |
| `deal_sensitivity_score` | `(avg_dollar_saved_per_km - MIN(avg_dollar_saved_per_km)) / (MAX(avg_dollar_saved_per_km) - MIN(avg_dollar_saved_per_km))` | Composite index reflecting user responsiveness to price benefits (0-1 scale) |
| `lifetime_customer_value` | `avg_cust_lifespan * customer_value_per_trip` | Estimate combining customer lifespan and average trip revenue |

## 📕 Customer Segments

### 📌 Segment Definitions

| Segment | Primary Criteria | Business Value |
|---------|------------------|----------------|
| **Luxury Travelers** | `customer_value_per_trip >= 2000 AND overall_transaction_value > 0` | Highest revenue per customer with premium service expectations |
| **Business Travelers** | `avg_nights_booked <= 2.5 AND num_trips >= 3 AND flight_hotel_mix_ratio >= 1.2 AND overall_transaction_value > 0` | High lifetime value with predictable booking patterns |
| **Family Travelers** | `(has_children = 1 OR (avg_nights_booked >= 4 AND mean_room_quantity >= 1.3)) AND overall_transaction_value > 0` | High booking volume with seasonal predictability |
| **Young Travelers** | `customer_age <= 30 AND overall_transaction_value > 0` | Future high-value customers with strong social influence |
| **Spontaneous Adventurers** | `booking_lead_interval <= 7 AND conversion_ratio >= 0.35 AND overall_transaction_value > 0` | High engagement potential with immediate gratification marketing |
| **Budget Travelers** | `customer_value_per_trip <= 800 AND promo_booking_ratio >= 0.25 AND overall_transaction_value > 0` | Volume-driven revenue with loyalty through value proposition |
| **Lost Opportunities** | `conversion_ratio <= 0.15 OR cancelled_session_count >= 1` | Recovery potential with targeted intervention strategies |

## 📕 Data Quality Terms

### 📌 Data Processing Concepts

| Term | Definition |
|------|------------|
| **Cohort** | Subset of users meeting specific criteria: >7 sessions after January 5, 2023 |
| **Session** | Single user interaction period on the platform, measured from start to end timestamp |
| **Trip** | Unique travel instance that may include flight and/or hotel bookings |
| **Binary Encoding** | Conversion of string boolean values ('true'/'false') to numeric format (1/0) |
| **Cleaned Nights** | Corrected hotel stay duration after fixing reversed check-in/check-out timestamps |
| **Haversine Distance** | Geospatial calculation method for flight distances using latitude/longitude coordinates |

### 📌 Data Quality Issues

| Issue | Description | Resolution |
|-------|-------------|-----------|
| **Negative Hotel Nights** | 12,067 records with check_out_time < check_in_time | Timestamp reordering logic applied |
| **Missing Return Times** | 88,734 flights without return_time (one-way flights) | Handled with NULL checks in calculations |
| **Zero Flight Distances** | Flights with distances <20km (likely data errors) | Flagged as anomalies, included with COALESCE |
| **Discount Anomaly** | 100% of discounted bookings resulted in cancellations | Critical system issue requiring investigation |

## 📕 Business Terms

### 📌 Marketing Concepts

| Term | Definition |
|------|------------|
| **Perk Assignment** | Segment-specific benefits and rewards tailored to customer preferences |
| **Conversion Rate** | Percentage of sessions that result in completed bookings |
| **Customer Lifetime Value** | Predicted total revenue from a customer over their entire relationship |
| **Retention Rate** | Percentage of customers who continue using the platform over time |
| **Churn** | Customer discontinuation of platform usage |

### 📌 Travel Industry Terms

| Term | Definition |
|------|------------|
| **MICE** | Meetings, Incentives, Conferences, and Exhibitions - business travel segment |
| **Lead Time** | Time between booking and travel date |
| **Base Fare** | Flight price before taxes, fees, and discounts |
| **One-way vs Round-trip** | Single direction travel vs. return journey booking |
| **Checked Bags** | Luggage stored in aircraft cargo hold (vs. carry-on) |

## 📕 Technical Terms

### 📌 SQL Functions & Operations

| Function | Purpose |
|----------|---------|
| `COALESCE` | Returns first non-null value, used for handling missing data |
| `NULLIF` | Returns NULL if expressions are equal, prevents division by zero |
| `EXTRACT(EPOCH FROM interval)` | Converts time interval to seconds |
| `OVER (PARTITION BY)` | Window function for group-based calculations |
| `CASE WHEN` | Conditional logic for data transformation |

### 📌 Analysis Concepts

| Term | Definition |
|------|------------|
| **Feature Engineering** | Process of creating meaningful variables from raw data for analysis |
| **Segmentation** | Division of customers into distinct groups based on behavioral patterns |
| **A/B Testing** | Experimental method comparing two versions to measure effectiveness |
| **Bias Injection** | Introduction of assumptions or external knowledge that may skew analysis |
| **Hierarchical Classification** | Priority-based segment assignment where higher-value segments take precedence |

## 📕 Age Group Classifications

| Age Group | Age Range | Characteristics |
|-----------|-----------|----------------|
| **< 18** | Under 18 | Minor travelers, likely family-dependent |
| **Student** | 18-26 | Young adults, budget-conscious, experience-focused |
| **Young Aged** | 27-34 | Early career, growing disposable income |
| **Middle Aged** | 35-60 | Peak earning years, family responsibilities |
| **Senior** | 60+ | Retired or nearing retirement, leisure-focused |

## 📕 Hotel Brand Categories

### 📌 Luxury Brands
Aman, Banyan Tree, Four Seasons, Rosewood, Shangri-La, Fairmont, Conrad, Park Hyatt, Ritz-Carlton

### 📌 Business Brands
Hilton, Marriott, NH Hotels, Crowne Plaza, Hyatt, InterContinental, Accor

### 📌 Budget Brands
Extended Stay, Choice Hotels, Best Western, Wyndham, Moxy, Accor (Ibis)

### 📌 Family-Friendly Brands
Marriott (Residence Inn), Hilton (Homewood Suites), NH Hotels, Best Western, Wyndham, Extended Stay, Choice Hotels, Accor (Novotel)

**Note:** This glossary serves as the definitive reference for all terminology used throughout the customer segmentation project documentation.
