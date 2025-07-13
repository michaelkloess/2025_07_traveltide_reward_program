Feature Engineering - Devising Metrics
│
├── 01_engagement_metrics                         
│   ├── session_frequency                         ← COUNT(*) FROM session_combined_table
│   ├── first_seen_date                           ← MIN(session_start::DATE)
│   ├── last_seen_date                            ← MAX(session_end::DATE)
│   ├── cancelled_session_count                   ← SUM(binary_cancellation)
│   ├── distinct_trip_count                       ← COUNT(DISTINCT trip_id)
│   ├── active_presence_days                      ← COUNT(DISTINCT DATE(session_start))
│   ├── mean_clicks_per_session                   ← AVG(page_clicks)
│   └── average_session_time                      ← AVG(EXTRACT(EPOCH FROM (session_end - session_start)))
│
├── 02_booking_behavior_metrics                   
│   ├── completed_flight_orders                   ← SUM(CASE WHEN binary_flight_booked = 1 AND trip_cancelled = 0 THEN 1 ELSE 0 END)
│   ├── completed_hotel_orders                    ← SUM(CASE WHEN binary_hotel_booked = 1 AND trip_cancelled = 0 THEN 1 ELSE 0 END)
│   ├── aggregate_flight_spend                    ← SUM(COALESCE(base_fare_usd, 0))
│   ├── aggregate_hotel_spend                     ← SUM(COALESCE(hotel_price_per_room_night_usd, 0) * COALESCE(rooms, 0) * COALESCE(cleaned_nights, 0))
│   ├── overall_transaction_value                 ← aggregate_flight_spend + aggregate_hotel_spend
│   ├── trip_revenue_avg                          ← SUM(base_fare_usd + hotel_price_total) / COUNT(DISTINCT trip_id)
│   ├── session_revenue_avg                       ← SUM(base_fare_usd + hotel_price_total) / COUNT(session_id)
│   ├── user_activity_lifespan                    ← MAX(session_end::DATE) - MIN(session_start::DATE)
│   ├── mean_savings_value                        ← SUM(flight_savings + hotel_savings)
│   └── savings_per_kilometer                     ← SUM(flight_savings) / SUM(flown_flight_distance)
│
└── 03_trip_dynamics_metrics                      
    ├── conversion_ratio                          ← SUM(CASE WHEN binary_flight_booked = 1 OR binary_hotel_booked = 1 THEN 1 ELSE 0 END) / COUNT(session_id)
    ├── avg_nights_booked                         ← SUM(cleaned_nights) / COUNT(DISTINCT trip_id)
    ├── mean_room_quantity                        ← SUM(rooms) / COUNT(DISTINCT trip_id)
    ├── mean_seat_count                           ← SUM(seats) / COUNT(DISTINCT trip_id)
    ├── avg_flight_distance                       ← SUM(flown_flight_distance) / COUNT(DISTINCT trip_id)
    ├── avg_trip_timespan                         ← AVG(return_time::DATE - departure_time::DATE)
    ├── mean_checked_bags                         ← SUM(checked_bags) / COUNT(DISTINCT trip_id)
    ├── booking_lead_interval                     ← AVG(departure_time::DATE - session_end::DATE)
    ├── cancel_lead_interval                      ← AVG(CASE WHEN cancellation = 'true' THEN (departure_time::DATE - session_end::DATE) ELSE NULL END)
    ├── hotel_spend_after_discount                ← SUM(hotel_price_per_room_night_usd * hotel_discount_amount * rooms * cleaned_nights)
    ├── flight_spend_after_discount               ← SUM(base_fare_usd * flight_discount_amount * seats)
    ├── promo_booking_ratio                       ← SUM(CASE WHEN binary_flight_discount = 1 OR binary_hotel_discount = 1 THEN 1 ELSE 0 END) / COUNT(session_id)
    ├── checked_bag_usage_ratio                   ← SUM(checked_bags) / SUM(CASE WHEN binary_flight_booked = 1 THEN 1 ELSE 0 END)
    ├── flight_hotel_mix_ratio                    ← SUM(CASE WHEN binary_flight_booked = 1 THEN 1 ELSE 0 END) / SUM(CASE WHEN binary_hotel_booked = 1 THEN 1 ELSE 0 END)
    ├── deal_sensitivity_score                    ← (avg_dollar_saved_per_km - MIN(avg_dollar_saved_per_km)) / (MAX(avg_dollar_saved_per_km) - MIN(avg_dollar_saved_per_km))
    └── lifetime_customer_value                   ← avg_cust_lifespan * customer_value_per_trip
