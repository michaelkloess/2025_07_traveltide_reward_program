### Aggregating Session-Based Data to User Level

**Objective:**  
Generate a user-level dataset from session-based data sources.

**Description:**  
This step consolidates all available session-based information from the 'sessions', 'flights', and 'hotels' tables into a single user-level table.  
The resulting dataset will have:
- One row per unique 'user_id'
- Aggregated features such as:
  - Total number of sessions
  - Average session duration
  - Number of flight and hotel bookings
  - Conversion status and other behavioral indicators

**Target Structure:**

| user_id | num_sessions | avg_session_time | num_flights | num_hotels | converted | ... |
|---------|--------------|------------------|-------------|------------|-----------|-----|

The resulting dataset is saved in the `02_data/processed/` directory and used in subsequent analysis and modeling steps.



Data Pre Processing: 

Alle Booleans in Binär umgewandelt


Given the large volume of data in the session table, we have implemented a cohort filter as outlined below:

Sessions after 04th Jan 2023 Users with more than 7 sessions Regarding the "nights" column, we encountered issues with invalid or incorrect data, including values such as 0, -1, and -2.

In an effort to determine valid entries, we analyzed the check_in_time and check_out_time columns. However, we were unable to establish a consistent pattern:

♦ The check-in and check-out times were often recorded in reverse order (e.g., check-out time preceding check-in time, or vice versa).

♦ Additionally, the duration between check-in and check-out did not reliably correspond to the expected number of nights, indicating discrepancies in the data.

To address this, we decided to use the absolute values to correct the data inconsistencies.

Although it is impossible to determine the exact number of nights booked, we can reasonably assume that after a successful check-in at a hotel (none of the filtered records had NULL values in either the check-in or check-out columns), at least one night's payment must be made. Consequently, we replaced the invalid data with a value of 1.
