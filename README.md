<html>
<body>
  
# Uber-Fares-Dataset-Analysis-using-Power-BI
## Project Overview
This project explores the Uber Fares Dataset using Power BI to extract meaningful insights about fare distribution, ride patterns, and operational performance. The dataset was cleaned and enhanced in Python, followed by interactive data visualization and dashboard development in Power BI.

## Project Objectives
- To analyze Uber fare patterns,ride durations, and temporal trends
- To create new analytical features (e.g., hour, day, time categories)
- To develop an interactive Power BI dashboard showing:
  
### Fare distribution
 1. histograms
 2. box plots
### Ride patterns 
 1. Hourly
 2. Daily
 3. Monthly
    
### Seasonal and geographic trends
- To highlight busiest periods and, if possible, assess weather impact
- To deliver actionable business insights through data storytelling

## Methodology:

#### 🗃️ 1. Data Collection
- Downloaded Uber Fares dataset from Kaggle
- Included:
   1. fare amount
   2. pickup time
   3. geolocations

#### 🧹 2. Data Cleaning (Python)
- Used Pandas to load and inspect data
- Removed missing, duplicate, and outlier records
- Exported cleaned CSV for analysis in Power BI

#### 🧠 3. Feature Engineering
- Extracted hour, day, month, weekday
- Created time periods (Peak/Off-Peak), ride distance, and duration

#### 📊 4. Power BI Analysis
- Imported dataset into Power BI Desktop
- Built interactive visuals
 1. fare trends
 2. maps
 3. time series
- Added slicers, filters, and drill-down capabilities

## Analysis

### 📊 Analysis: Detailed Findings & Statistical Insights

#### 🚗 1. Fare Distribution
- Most fares fall between $5–$20
- Box plot revealed outliers above $100, likely long-distance or airport trips
- Peak hour fares tend to be slightly higher due to demand

#### ⏰ 2. Time-Based Ride Patterns
- Busiest hours: 7–9 AM and 5–7 PM (commute hours)
- Highest ride count on Fridays and Saturdays
- Monthly trends show increased activity during summer months

#### 📍 3. Geographic Trends
- Ride pickups are concentrated in central NYC
- Hotspots include Midtown Manhattan and Lower Manhattan
- Map visualization shows higher fares around airports and business districts

#### 📈 4. Correlations & Metrics
- Positive correlation between fare amount and distance
- Longer ride durations generally result in higher fares
- Average fare per km stays consistent but slightly rises during peak hours

</body>
</html>
