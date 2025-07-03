# 📄 Overview

This project uses a ***relational PostgreSQL database*** that forms the analytical foundation for TravelTide’s customer and booking data. </br>
The schema follows a typical star-like structure consisting of fact and dimension tables, capturing user profiles, session behavior, flight bookings, and hotel stays.

Each table is designed to support both descriptive analytics and personalization logic (e.g., for customer rewards). </br>
While not all tables are fully normalized (3NF), the design supports performant analytical queries and simplified joins.

Below you’ll find: </br>

➤ A detailed schema description per table </br>
➤ An overview of key columns with types and meanings </br>
➤ Classification of dimensions vs. measures </br>
➤ Metadata overview (keys, normalization, row counts) </br>

---

# 📂 Table: users 

| Column Name        | Description                                       | Type        |
| ------------------ | ------------------------------------------------- | ------------|
| `user_id` 🔑       | Unique user ID                                    | int         |
| `birthdate`        | User date of birth                                | date        |
| `gender`           | User gender                                       | text        |
| `married`          | User marriage status                              | bool        |
| `has_children`     | Whether the user has children                     | bool        |
| `home_country`     | User’s resident country                           | text        |
| `home_city`        | User’s resident city                              | text        |
| `home_airport`     | User’s preferred hometown airport                 | text        |
| `home_airport_lat` | Geographical north-south position of home airport | numeric     |
| `home_airport_lon` | Geographical east-west position of home airport   | numeric     |
| `sign_up_date`     | Date of TravelTide account creation               | date        |

# 📂 Table: sessions

| Column Name              | Description                          | Type      |
| ------------------------ | ------------------------------------ | --------- |
| `session_id` 🔑          | Unique browsing session ID           | text      | 
| `user_id` 🔑             | User ID                              | int       | 
| `trip_id` 🔑             | ID mapped to trip bookings           | text      | 
| `session_start`          | Start of session                     | timestamp |
| `session_end`            | End of session                       | timestamp |
| `flight_discount`        | Flight discount offered              | bool      |
| `hotel_discount`         | Hotel discount offered               | bool      |
| `flight_discount_amount` | Percentage off base fare             | numeric   |
| `hotel_discount_amount`  | Percentage off base night rate       | numeric   |
| `flight_booked`          | Whether flight was booked            | bool      |
| `hotel_booked`           | Whether hotel was booked             | bool      |
| `page_clicks`            | Number of clicks in session          | int       |
| `cancellation`           | Whether session was for cancellation | bool      |

# 📂 Table: flights 

| Column Name               | Description                      | Type      |
| ------------------------- | -------------------------------- | --------- |
| `trip_id` 🔑              | Unique trip ID                   | text      | 
| `origin_airport`          | User’s home airport              | text      | 
| `destination`             | Destination city                 | text      |
| `destination_airport`     | Airport in destination city      | text      |
| `seats`                   | Number of seats booked           | int       |
| `return_flight_booked`    | Return flight booked             | bool      |
| `departure_time`          | Departure time                   | timestamp |
| `return_time`             | Return time                      | timestamp |
| `checked_bags`            | Number of checked bags           | int       |
| `trip_airline`            | Airline name                     | text      |
| `destination_airport_lat` | Latitude of destination airport  | numeric   |
| `destination_airport_lon` | Longitude of destination airport | numeric   |
| `base_fare_usd`           | Base fare in USD                 | numeric   |

# 📂 Table: hotels

| Column Name          | Description                             | Type      |
| -------------------- | --------------------------------------- | --------- |
| `trip_id` 🔑         | Unique trip ID                          | text      |
| `hotel_name`         | Hotel brand name                        | text      |
| `nights`             | Number of nights stayed                 | int       |
| `rooms`              | Number of rooms booked                  | int       |
| `check_in_time`      | Check-in time                           | timestamp |
| `check_out_time`     | Check-out time                          | timestamp |
| `hotel_per_room_usd` | Price per room per night (pre-discount) | numeric   |


# 🗄️ Table Overview
| Table             | sessions                                                  | flights                                             | hotels                                                  | users                                                                |
|-------------------|-----------------------------------------------------------|-----------------------------------------------------|---------------------------------------------------------|----------------------------------------------------------------------|
|                   |                                                           |                                                     |                                                         |                                                                      |
| Primary key?      | session_id                                                | trip_id                                             | trip_id                                                 | user_id                                                              |
| Dim/Fct?          | Fact                                                      | Fact                                                | Fact                                                    | Dimensions                                                           |
| How many rows?    | 13                                                        | 13                                                  | 7                                                       | 11                                                                   |
| How many columns? | 5408063                                                   | 1901038                                             | 1918617                                                 | 1020926                                                              |
| What is each row? | Daten zu Nutzer-Sessions, Rabatten und Buchungsverhalten. | Informationen zu gebuchten Flügen und Reisedetails. | Angaben zu Hotelbuchungen, Aufenthaltsdauer und Kosten. | Demografische Nutzerinformationen wie Alter, Geschlecht und Wohnort. |
| 3NF?              | No                                                        | No                                                  | Yes                                                     | No                                                                   |

# 🗄️ Schema Structure: Key Dimensions and Metrics by Table

|          | Dimensions              | Measures               |
|----------|-------------------------|------------------------|
| sessions | session_id              | session_start          |
|          | user_id                 | session_end            |
|          | trip_id                 | page_clicks            |
|          | flight_discount         | flight_discount_amount |
|          | flight_booked           | hotel_discount         |
|          | hotel_booked            | -                      |
|          | cancellation            | -                      |
| hotels   | trip_id                 | nights                 |
|          | hotel_name              | rooms                  |
|          | -                       | check_in_time          |
|          | -                       | check_out_time         |
|          | -                       | hotel_per_room_usd     |
| flights  | trip_id                 | seats                  |
|          | origin_airport          | departure_time         |
|          | destination             | return_time            |
|          | destination_airport     | checked_bags           |
|          | return_flight_booked    | base_fare_usd          |
|          | trip_airline            | -                      |
|          | destination_airport_lat | -                      |
|          | destination_airport_lon | -                      |
| users    | user_id                 | birthdate              |
|          | gender                  | sign_up_date           |
|          | married                 | -                      |
|          | has_children            | -                      |
|          | home_country            | -                      |
|          | home_city               | -                      |
|          | home_airport            | -                      |
|          | home_airport_lat        | -                      |
|          | home_airport_lon        | -                      |
