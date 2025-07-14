# 📕 Feature Engineering Documentation

**📌 Overview**
This document outlines the comprehensive feature engineering approach for customer segmentation analysis. Features are organized into three primary categories capturing different aspects of user behavior: engagement patterns, booking preferences, and travel characteristics.

## 📕 Feature Engineering Strategy

### 📌 Engineering Approach
- **User-Level Aggregation:** All features calculated at individual user level from session data
- **Temporal Scope:** Features computed across entire cohort period (Jan 5 - Jul 28, 2023)
- **Business Relevance:** Each metric designed to capture distinct behavioral patterns for segmentation
- **Statistical Robustness:** Metrics include both volume and rate-based calculations for balanced analysis

## 📕 Feature Categories

### 📌 01_engagement_metrics
**Purpose:** Measure user interaction depth and platform usage patterns

```
├── session_frequency                         ← Total number of recorded user sessions within the analysis timeframe
├── first_seen_date                           ← Earliest known interaction date for a given user
├── last_seen_date                            ← Most recent session timestamp found in the dataset
├── cancelled_session_count                   ← Number of user actions flagged as canceled
├── distinct_trip_count                       ← Count of unique travel instances associated with the user
├── active_presence_days                      ← Number of unique calendar days the user engaged with the platform
├── mean_clicks_per_session                   ← Average number of page interactions per recorded session
└── average_session_time                      ← Mean length of time spent per session by a user
```

**Business Value:** Identifies power users, casual browsers, and engagement quality patterns essential for retention strategies.

### 📌 02_booking_behavior_metrics
**Purpose:** Quantify spending patterns and financial value generation

```
├── completed_flight_orders                   ← Total count of non-canceled flight transactions
├── completed_hotel_orders                    ← Number of successful hotel bookings excluding any reversals
├── aggregate_flight_spend                    ← Sum of all flight-related expenditures by the user
├── aggregate_hotel_spend                     ← Total monetary value spent on hotel accommodations
├── overall_transaction_value                 ← Combined spending across both flight and hotel bookings
├── trip_revenue_avg                          ← Average income per trip derived from completed bookings
├── session_revenue_avg                       ← Mean revenue calculated per session based on related purchases
├── user_activity_lifespan                    ← Time span between a user's first interaction and their latest known activity
├── mean_savings_value                        ← Average cost reduction achieved across bookings
└── savings_per_kilometer                     ← Typical amount saved per kilometer of flight distance
```

**Business Value:** Enables high-value customer identification and revenue optimization targeting.

### 📌 03_trip_dynamics_metrics
**Purpose:** Capture travel preferences and behavioral characteristics

```
├── conversion_ratio                          ← Proportion of sessions or interactions that resulted in a booking
├── avg_nights_booked                         ← Mean number of hotel nights reserved per trip
├── mean_room_quantity                        ← Average number of rooms included in hotel bookings
├── mean_seat_count                           ← Typical number of flight seats reserved per booking
├── avg_flight_distance                       ← Mean travel distance for flights, computed using geospatial methods
├── avg_trip_timespan                         ← Typical duration of a trip from start to end
├── mean_checked_bags                         ← Average number of baggage units checked in per trip
├── booking_lead_interval                     ← Average time between booking confirmation and the travel start date
├── cancel_lead_interval                      ← Typical time lag between a booking date and its cancellation
├── hotel_spend_after_discount                ← Total cost incurred on hotels after discounts were applied
├── flight_spend_after_discount               ← Aggregated flight charges post-discount
├── promo_booking_ratio                       ← Share of bookings made when a promotion or discount was active
├── checked_bag_usage_ratio                   ← Rate at which users opted to check in luggage during air travel
├── flight_hotel_mix_ratio                    ← Proportional comparison between hotel and flight bookings per user
├── deal_sensitivity_score                    ← Composite index reflecting user responsiveness to price benefits
└── lifetime_customer_value                   ← Estimate combining customer lifespan and average trip revenue
```

**Business Value:** Enables precise travel persona identification for targeted marketing and product recommendations.

## 📕 Feature Engineering Methodology

### 📌 Calculation Approach
- **Aggregation Functions:** SUM, AVG, COUNT, MIN, MAX applied to session-level data
- **Ratio Calculations:** Proportion-based metrics for normalized comparisons
- **Temporal Features:** Date arithmetic for lifespan and interval calculations
- **Geospatial Calculations:** Haversine distance formula for flight distances

### 📌 Data Quality Measures
- **Null Handling:** COALESCE functions ensure zero values for missing data
- **Division Safety:** NULLIF prevents division by zero errors
- **Outlier Management:** Percentile-based capping for extreme values
- **Binary Encoding:** String boolean conversion to numeric format

### 📌 Feature Validation
- **Business Logic:** Each metric validated against domain knowledge
- **Statistical Distribution:** Range and distribution checks for anomalies
- **Correlation Analysis:** Feature independence assessment
- **Segmentation Relevance:** Direct alignment with marketing objectives

**Next Steps:** Apply engineered features to clustering algorithms for customer segmentation model development.
