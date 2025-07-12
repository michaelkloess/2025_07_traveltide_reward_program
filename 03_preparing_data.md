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
















