# Power BI Operations Dashboard  

![Dashboard Overview](assets/screenshot_overview.png)

A manufacturing-style operations dashboard that visualizes KPIs across **Production**, **Observations** (Safety & QA), and **Waste** using realistic synthetic data. Built to demonstrate practical Power BI skills: modeling, DAX, slicers, and visual storytelling.

## 📦 Data
Dataset used in this project:  
**ManufacturingData_Realistic.xlsx**

### Tables
- **Production**: `Date`, `Shift`, `Line`, `Units_Produced`, `Efficiency_Percent`, `Downtime_Minutes`  
- **Observations** (Safety + QA): `Date`, `Observation_Type`, `Department`, `Reported_By`, `Severity`  
- **Waste**: `Date`, `Waste_Type`, `Weight_Lbs`, `Disposed` (True/False)

> Note: Data are synthetic but tuned to be plausible (noise, seasonality, and realistic distributions).

## 🧱 Data Model
- **Dates** (central Date table, built in DAX)
- **Production** (fact table)
- **Observations** (fact table for Safety/QA)
- **Waste** (fact table)

**Relationships**:  
- `Dates[Date]` → `Production[Date]`  
- `Dates[Date]` → `Observations[Date]`  
- `Dates[Date]` → `Waste[Date]`  

All are **Many-to-One**, **Single direction**.

**DAX for Dates table:**
```DAX
Dates =
ADDCOLUMNS (
    CALENDAR ( DATE(2024,1,1), DATE(2024,12,31) ),
    "Year", YEAR ( [Date] ),
    "Month", FORMAT ( [Date], "MMM" ),
    "MonthNo", MONTH ( [Date] ),
    "Weekday", FORMAT ( [Date], "ddd" ),
    "WeekdayNo", WEEKDAY ( [Date], 2 )
)
🧮 Measures (DAX)
DAX
Copy code
-- Production
Efficiency % = AVERAGE ( Production[Efficiency_Percent] )
Units Produced = SUM ( Production[Units_Produced] )
Downtime (mins) = SUM ( Production[Downtime_Minutes] )

-- Observations
Total Observations = COUNTROWS ( Observations )

-- Waste
Total Waste (lbs) = SUM ( Waste[Weight_Lbs] )
Disposed % =
DIVIDE (
    CALCULATE ( COUNTROWS ( Waste ), Waste[Disposed] = TRUE ),
    COUNTROWS ( Waste )
)
Format Efficiency % as Percentage, Disposed % as Percentage.

📊 Core Visuals
Production Efficiency Over Time — Line chart

Observations by Department & Type — Stacked column chart

Waste by Type — Column chart

KPI Cards — Efficiency %, Units Produced, Downtime, Total Observations

Slicers — Date (from Dates table), Department, Shift, Line

🗺️ Layout
Header: Page title + slicers

KPI row: Efficiency %, Units Produced, Downtime, Total Observations

Left column (Production): Efficiency over time, Units over time

Right column (Observations/Waste): Observations by Department & Type, Waste by Type

📸 Screenshots
/assets/screenshot_overview.png — full dashboard

/assets/screenshot_production.png — Production focus

/assets/screenshot_observations.png — Observations focus

🧠 Skills Demonstrated
Power BI data modeling (star schema)

DAX measures for KPIs and ratios

Visual design (themes, slicers, KPI cards)

Storytelling with operational metrics

🔁 Future Enhancements
Add OEE calculation

Anomaly detection for downtime spikes

Row-level security (RLS) by department

Publish to Power BI Service