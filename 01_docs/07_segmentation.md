# 📕 Customer Segmentation Framework

**📌 Overview**
This document presents the comprehensive customer segmentation strategy derived from exploratory data analysis and business logic. Seven distinct segments have been identified, each with specific behavioral patterns, feature criteria, and targeted perk strategies for enhanced customer retention.

## 📕 Segmentation Methodology

### 📌 Development Approach
- **Data-Driven Foundation:** Segments based on quantitative feature analysis and behavioral patterns
- **Business Logic Integration:** Incorporates domain knowledge and marketing expertise
- **Actionable Design:** Each segment includes specific targeting criteria and perk recommendations
- **Revenue Optimization:** Segments aligned with customer lifetime value and retention potential

## 📕 Customer Segments

### 📌 Spontaneous Adventurers
**Profile:** Last-minute bookers seeking immediate travel gratification

```
├── Features
│    ├── booking_lead_interval <= 7              ← Books within 7 days of travel
│    ├── conversion_ratio >= 0.35                ← High booking conversion rate
│    ├── overall_transaction_value > 0           ← Must have completed transactions
│    └── [Excludes higher priority segments]     ← Applied after luxury/business/family/young classification
│
├── Behavior
│    ├── Short time span between session and departure
│    ├── Mostly single bookings (flight or hotel)
│    ├── High click volume per session, frequent use but irregular bookings
│    └── Bookings often in countries with low lead time (e.g., domestic European flights)
│
├── Preferred Destinations
│    ├── Cities: berlin, amsterdam, lisbon, barcelona, vienna, prague, rome, dubai
│    └── Hotels: Moxy, NH Hotels, Choice Hotels, Best Western, Wyndham
│
└── Perks
     ├── Digital spin-the-wheel with surprise discounts
     ├── Last-minute bundles
     ├── Instant booking bonus
     ├── Gamified "3-click trip" experiences
     ├── Exclusive weekend flash deals
     ├── Mystery trip option (destination revealed later)
     ├── Instant reward points after booking
     ├── Free cancellation
     ├── Push notifications for time-sensitive deals
     └── Combo benefits for spontaneous flight + hotel bookings
```

**Business Value:** High engagement potential with immediate gratification marketing strategies

### 📌 Business Travelers
**Profile:** Professional travelers with structured booking patterns and higher spend

```
├── Features
│    ├── avg_nights_booked <= 2.5                ← Short business trip durations
│    ├── num_trips >= 3                          ← Multiple trips indicating frequent travel
│    ├── flight_hotel_mix_ratio >= 1.2           ← Flight-heavy booking preference
│    ├── overall_transaction_value > 0           ← Must have completed transactions
│    └── [Excludes luxury travelers]             ← Applied after luxury classification
│
├── Behavior
│    ├── Frequent short trips during weekdays
│    ├── Flights and hotels are booked together, often with return flight
│    ├── Early planning and booking
│    └── Destinations are business hubs or MICE hotels
│
├── Preferred Destinations
│   ├── Cities: london, frankfurt, brussels, paris, vienna, geneva, zurich, singapore, new york
│   └── Hotels: Hilton, Marriott, NH Hotels, Crowne Plaza, Hyatt, InterContinental, Accor
│
└── Perks
     ├── Subscription model with pricing benefits
     ├── Business lounge access
     ├── More flexible cancellation policies
     ├── Calendar sync for trip planning
     ├── Miles/points program
     ├── Express check-in via app
     ├── Airport shuttle service
     ├── Travel expense PDF export
     ├── Priority support
     └── Stable Wi-Fi
```

**Business Value:** High lifetime value with predictable booking patterns and premium service expectations

### 📌 Luxury Travelers
**Profile:** High-spend customers prioritizing premium experiences over price sensitivity

```
├── Features
│    ├── customer_value_per_trip >= 2000          ← Premium spending threshold
│    ├── overall_transaction_value > 0            ← Must have completed transactions
│    └── [Highest priority segment]               ← Applied first in classification hierarchy
│
├── Behavior
│    ├── High spend on flights and hotels
│    ├── Few sessions, quick decision-making
│    ├── Rare use of discounts
│    └── Focus on comfort and brand status
│
├── Preferred Destinations
│    ├── Cities: paris, florence, tokyo, vienna, singapore, new york, dubai
│    └── Hotels: Aman, Banyan Tree, Four Seasons, Rosewood, Shangri-La, Fairmont, Conrad, InterContinental, Park Hyatt, Ritz-Carlton
│
└── Perks
     ├── Concierge service via app
     ├── Business lounge access
     ├── VIP support (phone + chat)
     ├── Private transfer included
     ├── Priority check-in & boarding
     ├── Exclusive events at destination
     ├── Sustainability points with premium charity options
     ├── Gift at booking (e.g. wine, wellness voucher)
     ├── Monthly "Luxury Surprise Drop"
     └── Premium partner perks (streaming, lifestyle products)
```

**Business Value:** Highest revenue per customer with brand loyalty potential and premium service expectations

### 📌 Family Travelers
**Profile:** Multi-generational travelers requiring family-focused accommodations and services

```
├── Features
│    ├── has_children = 1                         ← Primary family identifier
│    ├── OR (avg_nights_booked >= 4 AND mean_room_quantity >= 1.3)  ← Alternative family indicators
│    ├── overall_transaction_value > 0            ← Must have completed transactions
│    └── [Applied after luxury/business classification]  ← Third in hierarchy
│
├── Behavior
│    ├── Visit cities with family activities - Disneyland, theme parks
│    ├── More rooms or longer stays
│    └── Combined bookings with fixed dates (school holidays)
│ 
├── Preferred Destinations
│    ├── Cities: orlando, barcelona, lisbon, rome, edinburgh, dubai, bangkok
│    └── Hotels: Marriott (Residence Inn), Hilton (Homewood Suites), NH Hotels, Best Western, Wyndham, Extended Stay, Choice Hotels, Accor (Novotel)
│
└── Perks
     ├── Kids stay free
     ├── Family bundle deals
     ├── Child-friendly app content
     ├── Cheaper family travel insurance
     ├── Curated family accommodations
     ├── School holiday reminder tool
     ├── Theme park discounts
     ├── "Parents need a break" surprises
     ├── Family travel checklists
     └── Emergency translator service
```

**Business Value:** High booking volume with seasonal predictability and multi-service requirements

### 📌 Budget Travelers
**Profile:** Price-sensitive customers maximizing value through discounts and promotions

```
├── Features
│    ├── customer_value_per_trip <= 800           ← Low spending threshold
│    ├── promo_booking_ratio >= 0.25              ← High discount utilization
│    ├── overall_transaction_value > 0            ← Must have completed transactions
│    └── [Applied after higher value segments]    ← Later in classification hierarchy
│
├── Behavior
│    ├── Long session durations, high clicks
│    ├── Discount- and voucher-driven
│    └── Bookings usually only flight or hotel
│
├── Preferred Destinations
│    ├── Cities: sofia, warsaw, budapest, istanbul, bangkok, kuala lumpur
│    └── Hotels: Extended Stay, Choice Hotels, Best Western, Wyndham, NH Hotels, Moxy, Accor (Ibis)
│
└── Perks
     ├── Instant discount with gift cards
     ├── Discounts for returning users
     ├── Budget-focused cross-selling
     ├── Price alerts for wishlist destinations
     ├── Free currency exchange
     ├── Gamification via discount points
     ├── Free random upgrade to 1st class
     ├── Cashback for specific behavior
     ├── Bonus for 3+ bookings per quarter
     └── Budget flight & hostel comparison bundles
```

**Business Value:** Volume-driven revenue with loyalty through value proposition and gamification

### 📌 Young Travelers
**Profile:** Millennial and Gen-Z travelers focused on experiences and social connectivity

```
├── Features
│    ├── customer_age <= 30                       ← Young demographic filter
│    ├── overall_transaction_value > 0            ← Must have completed transactions
│    └── [Applied after luxury/business/family classification]  ← Fourth in hierarchy
│
├── Behavior
│    ├── Age under 25
│    ├── Budget-focused
│    ├── Often group travel or hostels
│    ├── High engagement, social media active
│    └── Party destinations preferred
│
├── Preferred Destinations
│    ├── Cities: barcelona, prague, amsterdam, budapest, berlin, bangkok
│    └── Hotels: Moxy, Generator, NH Hotels, Choice Hotels, Wyndham, Best Western, Hyatt (Centric)
│
└── Perks
     ├── Digital spin-the-wheel
     ├── Discounts for partners (clubs, bars, parks)
     ├── Group booking discounts
     ├── Social media sharing bonus
     ├── Streaming platform discounts
     ├── Early access to flash deals
     ├── "Refer 3, travel free" deals
     ├── Discovery cards with small rewards
     ├── Free event tickets
     └── Gamified badges (e.g. "First Trip Alone")
```

**Business Value:** Future high-value customers with strong social influence and brand advocacy potential

### 📌 Lost Opportunities
**Profile:** High-engagement users with poor conversion requiring reactivation strategies

```
├── Features
│   ├── conversion_ratio <= 0.15                  ← Very low booking success
│   ├── OR cancelled_session_count >= 1           ← Users with cancellation history
│   └── [Catch-all for problematic users]         ← Applied regardless of transaction value
│
├── Behavior
│   ├── Frequent exploration, no booking
│   ├── Interest shown but no revenue
│   └── Possibly churned or inactive
│
├── Preferred Destinations
│   ├── Cities: all high-interest destinations above
│   └── Hotels: all major brands (broad interest, no commitment)
│
└── Perks
    ├── Reactivation campaigns
    ├── Targeted retention emails
    ├── Personal voucher codes
    ├── Special rebooking discounts
    ├── Surprise incentive after inactivity
    ├── Simplified rebooking journey
    ├── Personal callback service
    ├── Exit-intent popups
    ├── Behavior analysis insights
    └── "Come back" bonus offers
```

**Business Value:** Recovery potential with targeted intervention and personalized reengagement strategies

## 📕 Implementation Strategy

### 📌 Hierarchical Classification Logic
The segmentation follows a prioritized classification system:

1. **Luxury Travelers** (Highest Priority)
   - `customer_value_per_trip >= 2000 AND overall_transaction_value > 0`

2. **Business Travelers**
   - `avg_nights_booked <= 2.5 AND num_trips >= 3 AND flight_hotel_mix_ratio >= 1.2 AND overall_transaction_value > 0`

3. **Family Travelers**
   - `(has_children = 1 OR (avg_nights_booked >= 4 AND mean_room_quantity >= 1.3)) AND overall_transaction_value > 0`

4. **Young Travelers**
   - `customer_age <= 30 AND overall_transaction_value > 0`

5. **Spontaneous Adventurers**
   - `booking_lead_interval <= 7 AND conversion_ratio >= 0.35 AND overall_transaction_value > 0`

6. **Budget Travelers**
   - `customer_value_per_trip <= 800 AND promo_booking_ratio >= 0.25 AND overall_transaction_value > 0`

7. **Lost Opportunities**
   - `conversion_ratio <= 0.15 OR cancelled_session_count >= 1`

8. **Others** (Default)
   - All remaining users not classified in above segments

### 📌 Segmentation Application
- **Feature-Based Classification:** Automated segment assignment using defined criteria
- **Dynamic Updates:** Regular re-segmentation based on evolving user behavior
- **Perk Personalization:** Targeted benefit delivery aligned with segment preferences
- **Performance Monitoring:** Conversion and retention tracking by segment

### 📌 Success Metrics
- **Segment Stability:** Consistent user classification over time
- **Conversion Improvement:** Increased booking rates within targeted segments
- **Customer Lifetime Value:** Enhanced revenue per segment through personalized perks
- **Retention Enhancement:** Reduced churn through relevant engagement strategies

**Next Steps:** Implement rule-based segmentation logic and deploy personalized perk assignment system for targeted customer engagement.
