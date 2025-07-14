# 📕 Cohort Definition Documentation

**📌 Overview**
This document defines the analytical cohort selected for customer segmentation analysis in collaboration with Elena, Head of Marketing. The cohort focuses on high-engagement users during a specific timeframe to ensure reliable behavioral insights.

## 📕 Cohort Parameters

### 📌 Temporal Scope
- **Full Database Range:** April 2021 - July 2023
- **Selected Analysis Period:** January 5, 2023 - July 28, 2023
- **Duration:** 6 months and 24 days (205 days total)

**Rationale:** Elena's recommendation to focus on recent user behavior patterns while ensuring sufficient data volume for statistically significant segmentation.

### 📌 User Activity Filter
- **Minimum Session Threshold:** >7 sessions per user
- **Inclusion Criteria:** Users must exceed 7 sessions within the analysis period
- **Exclusion:** Low-engagement users with ≤7 sessions

## 📕 Cohort Justification

### 📌 Temporal Selection Benefits
- **Recency:** Captures current user behavior patterns
- **Data Quality:** Avoids historical system changes and COVID-19 anomalies
- **Sufficient Volume:** 6+ months provides adequate behavioral data
- **Business Relevance:** Aligns with recent marketing initiatives

### 📌 Session Threshold Rationale
- **Engagement Focus:** Targets committed platform users
- **Behavioral Reliability:** Sufficient interactions for pattern recognition
- **Segmentation Validity:** Reduces noise from casual browsers
- **Business Value:** Concentrates on users with retention potential

## 📕 Potential Considerations

### 📌 Session Threshold Evaluation
**Current Approach:** Fixed threshold of >7 sessions
**Alternative Considerations:**
- **Session Duration Filter:** Exclude sessions <30 seconds to remove bounces
- **Quality-based Metrics:** Consider page clicks or booking attempts
- **Adaptive Thresholds:** Vary by user registration date

**Recommendation:** Validate current threshold against business objectives and user value metrics

### 📌 Bias Mitigation Opportunities
**Identified Risks:**
- **Short Session Bias:** Very brief sessions may skew behavioral analysis
- **Threshold Arbitrariness:** 7-session cutoff may exclude valuable user segments
- **Temporal Bias:** Recent period may not represent long-term patterns

**Proposed Enhancements:**
- Session quality filters (duration >30 seconds)
- Sensitivity analysis with different thresholds (5, 10, 15 sessions)
- Cohort comparison with different time periods

## 📕 Cohort Characteristics

### 📌 Expected Cohort Profile
- **User Type:** High-engagement, repeat visitors
- **Behavioral Depth:** Multiple booking explorations or completions
- **Segmentation Readiness:** Sufficient data points for reliable clustering
- **Business Relevance:** Primary target for retention programs

### 📌 Analytical Implications
- **Segment Stability:** Reduced volatility from casual users
- **Pattern Recognition:** Clear behavioral signals for segmentation
- **Marketing Applicability:** Cohort represents core retention target
- **Model Reliability:** Adequate data volume per user for feature engineering

## 📕 Implementation Notes

### 📌 Data Processing
- Filter: `session_start >= '2023-01-05'`
- User qualification: `COUNT(sessions) > 7`
- Analysis period: 205-day window
- Expected cohort size: Subset of total user base with high engagement

### 📌 Quality Assurance
- Validate session counting methodology
- Review temporal boundary accuracy
- Confirm user inclusion/exclusion logic
- Monitor cohort size and representativeness

**Next Steps:** Proceed with user aggregation and feature engineering on defined cohort, with option to refine parameters based on initial segmentation results.
