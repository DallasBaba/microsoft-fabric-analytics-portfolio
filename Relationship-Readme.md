# Data Modeling Design

## 1) Overview

This project implements a fact centric star schema optimized for analytics in Microsoft Fabric Power BI, supporting both operational monitoring and executive KPI reporting.

The model is designed to:
- Keep facts at their natural grain (trip-level, load-level, event-level, monthly-level)
- Use conformed dimensions (Driver, Truck, Customer, Route, Facility, Date)
- Avoid ambiguous filter paths via single-direction relationships
- Encourage reusable, enterprise-style measures (time intelligence, ranking, profitability, utilization)

## 2) Model Reference (Image 1 & Image 2)

### Image1  Relationship Diagram 
<img width="618" height="298" alt="image" src="https://github.com/user-attachments/assets/c9583ca1-7563-4a74-a808-594599e83c2a" />



Image 1 shows the semantic model layout with:
- **`fact_trips`** as the center “spine”
- Shared dimensions connected one-to-many (1:*)
- Additional facts (fuel, maintenance, safety, delivery events, monthly metrics) connected through natural keys (`driver_id`, `truck_id`, `trip_id`, and date/month fields)

### Image2 Table/Column attributes
<img width="681" height="336" alt="image" src="https://github.com/user-attachments/assets/b4180dbe-cf4f-4cfe-a0c0-a96d02f083c1" />


Image 2 confirms:
- Dimension tables contain descriptive attributes (names, types, locations, acquisition dates, etc.)
- Fact tables contain measurable operational/financial fields (miles, revenue, gallons, costs, downtime, on-time flags, claims)

> Together, these two views reflect a **star schema with a trip-centric operational core** and domain-specific facts layered around it.

## 3) Modeling Approach

### 3.1 Star Schema with Conformed Dimensions
The model uses shared dimensions across multiple fact tables so the same slicers work everywhere (time, driver, truck, customer, route).

**Benefits**
- Cross-domain analysis (like: Revenue vs Safety vs Maintenance)
- Reusable KPIs
- Strong performance behavior in Power BI 

## 4) Why `fact_trips` is the Central Fact

### 4.1 Business Reason
A trip is the most consistent operational unit that connects:
- Driver assignment
- Truck and trailer usage
- Load movement and route behavior
- Distance, duration, fuel usage, and performance outcomes

### 4.2 Modeling Reason
`fact_trips` is ideal as the core because it:
- Provides stable keys (`trip_id`, `driver_id`, `truck_id`, `trailer_id`, `load_id`)
- Supports clean joins to dimensions
- Enables consistent “per trip / per mile / per hour” normalization

### 4.3 Analytical Reason
Most executive KPIs can be attributed at the trip grain:
- Revenue per mile, cost per mile
- MPG, idle time, utilization rate
- On-time rate and detention impact
- Safety risk and incident rates

## 5) Relationship Design Decisions

### 5.1 One-to-Many (1:*) Relationships
All relationships are modeled as:
- Dimensions on the 1-side
- Facts on the many-side

This ensures predictable filter propagation and correct aggregations.

### 5.2 Single-Direction Filtering
Relationships are intentionally **single direction** from Dimensions → Facts to:
- Prevent ambiguous paths
- Reduce accidental double-counting
- Keep measure logic explicit

### 5.3 Date Modeling (`Dim_Date`)
A dedicated `Dim_Date` table supports:
- Month/quarter/year slicing
- YoY, rolling periods, trend analysis

**Best practice**
> Only one active relationship from each fact to `Dim_Date`. If multiple date fields exist (dispatch vs delivery vs incident), keep one active and use `USERELATIONSHIP()` in measures when needed.

## 6) Fact Tables & Analytical Purpose

**Core Operational Facts**
- **`fact_trips`**: operational spine for mileage, time, assignment, performance
- **`fact_loads`**: demand + revenue by customer and route
- **`fact_delivery_events`**: on-time + detention + event-based performance

**Cost & Risk Facts**
- **`fact_fuel_purchases`**: gallons, price per gallon, fuel cost
- **`fact_maintenance_records`**: labor/parts/total cost + downtime
- **`fact_safety_incidents`**: claims, injury, preventable flags, severity costs

**Monthly Snapshot Facts**
- **`fact_driver_monthly_metrics`**: stable monthly performance (driver KPI view)
- **`fact_truck_utilization_metrics`**: monthly utilization, idle, downtime, mpg

## 7) Measure Patterns  

> The goal is **clarity + reusability**: measures should be readable, consistent, and designed to scale.

### 7.1 Base Measures  

```DAX
Total Revenue :=
SUM ( fact_loads[revenue] )
```

```DAX
Total Miles :=
SUM ( fact_trips[actual_distance_miles] )
```

```DAX
Total Fuel Cost :=
SUM ( fact_fuel_purchases[total_cost] )
```

```DAX
Total Maintenance Cost :=
SUM ( fact_maintenance_records[total_cost] )
```
### 7.2 Normalized Performance Measures (Per Mile / Per Trip / Per Hour)

```DAX
Revenue per Mile :=
DIVIDE ( [Total Revenue], [Total Miles] )
```

```DAX
Fuel Cost per Mile :=
DIVIDE ( [Total Fuel Cost], [Total Miles] )
```

```DAX
Maintenance Cost per Mile :=
DIVIDE ( [Total Maintenance Cost], [Total Miles] )
```

### 7.3 Profitability & Margin Measures
```DAX
Safety Cost = 
SUM ( fact_safety_incidents[cargo_damage_cost] )  + SUM ( fact_safety_incidents[claim_amount] )
```

```DAX
Route Profit :=
[Total Revenue] - [Total Fuel Cost]  - [Detention Cost] - [safey Cost]
```

```DAX
Profit Margin % :=
DIVIDE ( [Route Profit], [Total Revenue] )
```

### 7.4 Time Intelligence Measures (YTD / YoY / Rolling)

```DAX
Revenue YTD :=
TOTALYTD ( [Total Revenue], Dim_Date[Date] )
```

```DAX
Revenue YoY % :=
VAR PrevYear =
    CALCULATE ( [Total Revenue], SAMEPERIODLASTYEAR ( Dim_Date[Date] ) )
RETURN
DIVIDE ( [Total Revenue] - PrevYear, PrevYear )
```

```DAX
Revenue Rolling 3M :=
CALCULATE (
    [Total Revenue],
    DATESINPERIOD ( Dim_Date[Date], MAX ( Dim_Date[Date] ), -3, MONTH )
)
```
### 7.5 Ranking Measures (Dense Rank + Tie Handling)
 
```DAX
Rev/Mile Rank = 
VAR CurRevPM = ROUND ( [Revenue per Mile], 2 )
RETURN
IF (
    ISBLANK ( CurRevPM ),
    BLANK (),
    RANKX (
        ALLSELECTED ( dim_driver[driver_id] ),
        ROUND ( [Revenue per Mile], 2 ),
        CurRevPM,
        DESC,
        SKIP
    )
)
```

### 7.6 Utilization Measures

```DAX
Utilization Rate :=
AVERAGE ( fact_truck_utilization_metrics[utilization_rate] )
```

```DAX
Idle Time Hours :=
SUM ( fact_truck_utilization_metrics[idle_time_hours] )
```

```DAX
Downtime Hours :=
SUM ( fact_truck_utilization_metrics[downtime_hours] )
```

```DAX
Miles per Active Hour :=
VAR ActiveHours =
    SUM ( fact_truck_utilization_metrics[actual_duration_hours] )
RETURN
DIVIDE ( [Total Miles], ActiveHours )
```
### 7.7 On-Time Performance & Detention (2-hour free wait)



```DAX
Billable Detention Hours :=
VAR DetentionMinutes = SUM ( fact_delivery_events[detention_minutes] )
VAR DetentionHours   = DIVIDE ( DetentionMinutes, 60 )
VAR FreeHours        = 2
RETURN
MAX ( 0, DetentionHours - FreeHours )
```

```DAX
Detention Cost :=
[Billable Detention Hours] * [Detention Rate per Hour]
```

## 8) Industry Best Practices Used

- Single source of truth via curated Gold layer
- No many-to-many by default (bridge tables only if needed)
- Measure-driven logic instead of calculated columns where possible
- Conformed dimensions for cross-domain insights
- Dense ranking to handle ties correctly

## 9) Future Enhancements

- Cost allocation/bridge table for accessorials, detention payouts, claims
- Calculation groups for time intelligence (advanced modeling)
- Row-Level Security (RLS) by driver/facility/customer
- Snapshot modeling for historical status tracking


## 10) Summary

This model is designed to be **enterprise-friendly, performance-aware, and easy to explain**.  
By anchoring analysis on `fact_trips` and layering domain facts around shared dimensions, it supports profitability, utilization, benchmarking, and operational risk analytics.
