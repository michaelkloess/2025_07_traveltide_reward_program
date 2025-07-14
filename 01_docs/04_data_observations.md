# 📕 Data Observations Documentation

**📌 Overview**
This document presents key observations from exploratory data analysis of the travel booking platform, highlighting data quality issues, user behavior patterns, and business insights across users, sessions, flights, and hotels tables.

## 📕 Data Quality Assessment

### 📌 Users Table
- **1,020,926 entries** with 11 columns
- **No missing values** or duplicates detected
- **Quality Status**: No data cleaning required

**Key Distributions:**
- Gender: F (89%), M (11%), O (0%)
- Home Countries: USA, Canada
- Geographic Coverage: 105 cities, 159 airports

### 📌 Sessions Table
- **5,408,063 total sessions** with 13 columns
- **3,072,218 sessions without trip_id** (57% browsing only)
- **2,335,845 sessions with bookings** (43% conversion)

**Discount Patterns:**
- Flight discounts: 885,796 applied (16%)
- Hotel discounts: 691,380 applied (13%)

### 📌 Flights Table
- **1,901,038 entries** with 13 columns
- **88,734 missing return times** (one-way flights)
- **No duplicates detected**

**Data Anomalies:**
- Extremely short flight distances (<20km)
- 9,851 flights with identical departure and return times
- Peak departure time: 7:00 AM (5x higher than 8:00 AM)

### 📌 Hotels Table
- **1,918,617 entries** with 7 columns
- **No missing values** or duplicates
- **12,067 negative night values** (corrected via timestamp reordering)

**Corrected Anomalies:**
- 38,483 stays under 2 hours (potential test bookings/cancellations)
- Uniform distribution: 20 hotels per city (unusual market structure)

## 📕 Critical Business Observations

### 📌 Cancellation Crisis
**Observation:** All 90,670 cancellations involved both flight and hotel discounts

| Year | Cancellations |
|------|---------------|
| 2021 | 3,132         |
| 2022 | 32,700        |
| 2023 | 54,838        |

**Interpretation:** Discount mechanism appears fundamentally broken - discounts correlate with 100% cancellation rate rather than loyalty

**Business Impact:** Critical system failure requiring immediate investigation

### 📌 User Growth Analysis

| Year | New Users | Growth Rate  |
|------|-----------|--------------|
| 2021 | 75,555    | Baseline     |
| 2022 | 427,441   | +465.84%     |
| 2023 | 517,930   | +21.16%      |

**Observation:** Explosive 2022 growth followed by normalization
**Interpretation:** Temporary market expansion (possibly COVID recovery) without sustained retention

### 📌 Pricing Anomalies

**One-way vs Round-trip Pricing:**
- One-way flights: $306 average
- Round-trip flights: $661 average

**Interpretation:** Round-trip bookings are nearly double the price, contradicting typical airline pricing models

### 📌 User Behavior Patterns

**Session Duration Distribution:**
- 64% of sessions: ≤5 minutes
- Peak activity: 6-9 PM daily
- Consistent patterns across weekdays

**Cancellation Demographics:**
- Users without children: 75% of cancellations
- Gender distribution: Equal cancellation rates
- Time patterns: Higher cancellations during peak hours

### 📌 Market Positioning Issues

**Hotel Portfolio Analysis:**
- Target segments: Business travelers, luxury travelers
- **Gap:** Limited budget and family options
- **Risk:** Underserved segments may seek competitors

**Hotel Brands by Segment:**
- **Luxury:** Conrad, Fairmont, Four Seasons, Ritz-Carlton
- **Business:** Hilton, Marriott, InterContinental, Hyatt
- **Budget:** Best Western, Choice Hotels (limited presence)

## 📕 Data Quality Recommendations

### 📌 Immediate Actions Required

1. **Discount System Audit**
   - Investigate why all discounted bookings result in cancellations
   - Review discount application logic and payment processing

2. **Data Validation**
   - Implement checks for flight distances <20km
   - Validate departure/return time equality cases
   - Review hotel stay duration calculations

3. **Market Coverage Analysis**
   - Assess hotel portfolio gaps for budget/family segments
   - Evaluate geographic coverage optimization

### 📌 Monitoring Metrics

- Cancellation rate by discount status
- Session-to-booking conversion rates
- User retention across growth periods
- Geographic booking distribution

**Next Steps:** Focus segmentation analysis on high-value, non-discounted users to avoid contaminated discount data influencing customer personas.
