 <html>
<body>
<a href="https://colab.research.google.com/drive/1L6EUlEwzX-6mc6YbqK7XBb0N0LYo0Bm4" target="_blank">
  <img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"/>
</a>
 
# Visual Insights from Uber Fares Dataset
## 🚕 Project Overview
This project analyzes the Uber Fares Dataset with Power BI to uncover valuable insights into fare trends, ride behaviors, and operational metrics. Data preprocessing and enrichment were performed in Python, followed by the creation of interactive visualizations and dashboards using Power BI.

## 🎯 Project Objectives
- Examine Uber fare trends, ride durations, and time-based patterns
- Engineer new analytical features such as trip hour, day of the week, and time segments
- Design and build an interactive Power BI dashboard that visualizes key insights
  
### 💵 Fare distribution
 1. Histograms
 2. Box plots
### 🚗 Ride patterns 
1. Hourly level – showing peak hours and fare fluctuations
2. Daily level – identifying day-wise trends and ride volume
3. Monthly level – tracking long-term patterns and seasonal variations
    
### 🌦️ Seasonal and 🌍 geographic trends
- Highlight the busiest ride periods and, where applicable, evaluate the influence of weather conditions
- Deliver actionable business insights through engaging data storytelling and visualization

## Methodology:

#### 🗃️ 1. Data Collection
- Downloaded Uber Fares dataset from Kaggle
- Included:
   - fare amount
   - pickup time
   - geolocations

#### 🧹 2. Data Cleaning (Python)
- Used Pandas to load and inspect data
  ```python
  from google.colab import drive
  drive.mount('/content/drive')
  import pandas as pd
  uber_df = pd.read_csv('drive/MyDrive/archives/uber.csv', low_memory=False)
  # Check column data types
  uber_df.dtypes
  # Data set Structure
  uber_df.head()
  # Check the structure (columns, data types, non-null counts)
  uber_df.info()
  # Check the shape (rows, columns)
  uber_df.shape
  uber_df.describe()
<img width="914" height="158" alt="image" src="https://github.com/user-attachments/assets/f895a78e-9494-4711-9170-ad2a6d9fa1a9" />

<img width="925" height="212" alt="image" src="https://github.com/user-attachments/assets/f29d2a72-c0d6-4f7c-91d2-6e6922cd7956" />

<img width="923" height="170" alt="image" src="https://github.com/user-attachments/assets/d605b17c-e4d7-46f4-b719-2cbd8a9e85a4" />

- Removed missing, duplicate, and outlier records
 ```python
   # Check for missing values
   uber_df.isnull()
   uber_df.isnull().sum()
  # Check for duplicate rows
  print("Duplicate Rows:", uber_df.duplicated().sum())
  uber_df.duplicated()
  # Drop rows with any missing values 
  df = uber_df.dropna(axis=0)
 ```
<img width="923" height="239" alt="image" src="https://github.com/user-attachments/assets/751d2bca-71f3-4ba4-a233-7016c6e60f77" />

<img width="926" height="52" alt="image" src="https://github.com/user-attachments/assets/c42e7cda-cf58-4d2a-9a50-3ffc4fa7deef" />

<img width="919" height="207" alt="image" src="https://github.com/user-attachments/assets/c2fa454b-8eb8-4385-aac6-20f4f5fd4784" />

- Fare Distribution with Histogram and Box Plot in Google Colab(Fare amount vs Passenger count)
```python
# Fare amount vs passenger count
import matplotlib.pyplot as plt
import seaborn as sns
# 1. Histogram for fare_amount
plt.subplot(1, 2, 1)
sns.histplot(uber_df['fare_amount'], bins=30, kde=True)
plt.title('Fare Amount Distribution')
plt.xlabel('Fare Amount')
plt.ylabel('Frequency')

# 2. Boxplot of fare_amount grouped by passenger_count
plt.subplot(1, 2, 2)
sns.boxplot(x='passenger_count', y='fare_amount', data=uber_df)
plt.title('Fare Amount by Passenger Count')
plt.xlabel('Passenger Count')
plt.ylabel('Fare Amount')

plt.tight_layout()
plt.show()
```
<img width="935" height="329" alt="image" src="https://github.com/user-attachments/assets/6ba39f75-e732-4c44-830a-8eb071f97e25" />

-Fare Distribution with Histogram and Box Plot in Google Colab(Fare vs Distance)
```python
#  Fare amount vs. distance traveled
# Haversine function to calculate distance in kilometers
import numpy as np
def haversine(lat1, lon1, lat2, lon2):
    R = 6371  # Earth radius in km
    lat1, lon1, lat2, lon2 = map(np.radians, [lat1, lon1, lat2, lon2])
    dlat = lat2 - lat1
    dlon = lon2 - lon1
    a = np.sin(dlat/2.0)**2 + np.cos(lat1) * np.cos(lat2) * np.sin(dlon/2.0)**2
    c = 2 * np.arcsin(np.sqrt(a))
    return R * c

    # Calculate distance between pickup and dropoff
uber_df['distance_km'] = haversine(
    uber_df['pickup_latitude'],
    uber_df['pickup_longitude'],
    uber_df['dropoff_latitude'],
    uber_df['dropoff_longitude']
)

# Optional: filter out unrealistic distances or fares
uber_df = uber_df[(uber_df['distance_km'] > 0) & (uber_df['distance_km'] < 100) & (uber_df['fare_amount'] > 0) & (uber_df['fare_amount'] < 200)]

# Plot fare vs. distance
plt.figure(figsize=(10,6))
sns.scatterplot(x='distance_km', y='fare_amount', data=uber_df, alpha=0.5)
sns.regplot(x='distance_km', y='fare_amount', data=uber_df, scatter=False, color='red')  # trend line
plt.title('Fare Amount vs. Distance Traveled')
plt.xlabel('Distance (km)')
plt.ylabel('Fare Amount ($)')
plt.grid(True)
plt.tight_layout()
plt.show()
```
<img width="554" height="332" alt="image" src="https://github.com/user-attachments/assets/2cf66132-2fc7-4dfa-80d2-a28141d95af3" />

- Exported cleaned CSV for analysis in Power BI
  ```python
  # Save cleaned dataset
  from google.colab import files
  uber_df.to_csv('uber.csv', index=False)
  files.download('uber.csv')
  ```

#### 🧠 3. Feature Engineering
- Extracted hour, day, month, weekday
  ```python
  import pandas as pd
  import matplotlib.pyplot as plt
  import seaborn as sns
  
  # Ensure pickup_datetime is in datetime format (with timezone awareness removed or converted)
  uber_df['pickup_datetime'] = pd.to_datetime(uber_df['pickup_datetime'], utc=True)
  uber_df['pickup_datetime'] = uber_df['pickup_datetime'].dt.tz_convert(None)  # Convert to naive datetime if needed
  
  # Extract hour from pickup time
  uber_df['hour'] = uber_df['pickup_datetime'].dt.hour
  
  # Optional: Filter unrealistic fare amounts
  uber_df = uber_df[(uber_df['fare_amount'] > 0) & (uber_df['fare_amount'] < 200)]
  
  # Plot average fare by hour
  plt.figure(figsize=(10, 6))
  sns.lineplot(x='hour', y='fare_amount', data=uber_df, estimator='mean', ci=None, marker='o')
  plt.title('Average Fare Amount by Time of Day')
  plt.xlabel('Hour of Day (0 = Midnight)')
  plt.ylabel('Average Fare Amount ($)')
  plt.grid(True)
  plt.tight_layout()
  plt.show()
  ```
- Created time periods (Peak/Off-Peak), ride distance, and duration
  ```python
  import pandas as pd
  # Ensure datetime is parsed correctly (timezone-aware then convert to naive if needed)
  uber_df['pickup_datetime'] = pd.to_datetime(uber_df['pickup_datetime'], utc=True)
  uber_df['pickup_datetime'] = uber_df['pickup_datetime'].dt.tz_convert(None)
  
  # Create new features from datetime
  uber_df['hour'] = uber_df['pickup_datetime'].dt.hour
  uber_df['day'] = uber_df['pickup_datetime'].dt.day
  uber_df['month'] = uber_df['pickup_datetime'].dt.month
  uber_df['day_of_week'] = uber_df['pickup_datetime'].dt.day_name()  # e.g., Monday, Tuesday
  
  # Peak/Off-peak indicator
  # Let's assume peak hours are 7-9 AM and 4-7 PM
  def get_peak_hour(hour):
      if 7 <= hour <= 9 or 16 <= hour <= 19:
          return 'Peak'
      else:
          return 'Off-Peak'
  
  uber_df['time_period'] = uber_df['hour'].apply(get_peak_hour)
    ```

#### 📊 4. Power BI Analysis
- Imported dataset into Power BI Desktop
  <img width="959" height="478" alt="image" src="https://github.com/user-attachments/assets/489b1df1-5805-4ac2-99dc-e23bb968ecea" />

- Built interactive visuals
- DAX formulas used
   - Count
   - Sum
   - Average
   - Variance
   - Standard Variation
     
####  Fare trends <br>
##### Fare Amount by Hour
   ![alt text](pics/fare_amountbyhour.png)
##### Average Fair Amount by Year
   ![alt text](pics/averagefairamountbyyear.png)
####  Maps <br>
##### Latitudes and Longitudes
   ![alt text](pics/latitudeandlongitude.png)
####  Time series <br>
##### Pick up Date Time by Hour
   ![alt text](pics/pickupdatetimebyhour.png)
##### Sum of Hour by Year
   ![alt text](pics/sumofhourbyyear.png)
##### Pick up Date Time by Day
   ![alt text](pics/pickupdatetimenyday.png)
##### Pick up Date time by Month
   ![alt text](pics/pickupdatetimebymonth2.png)
#### Added slicers, filters, and drill-down capabilities
##### Slicers(Interactive Filters)
   ![alt text](pics/slicers.png)

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

## Key Results:
### Key Discoveries and Pattern Identification

#### 🕐 Peak ride hours
Ride volumes peak during morning (7–9 AM) and evening (5–7 PM) commute times.

#### 📅 Weekly Trends
Fridays and Saturdays have the highest ride activity, reflecting strong weekend demand.

#### 💰 Fare Patterns
Most fares fall between $5 and $20, with occasional outliers above $100 for longer trips.

#### 📍 Geographic Hotspots
Midtown and Lower Manhattan show the highest pickup density.

Airport areas tend to have higher fares and longer trip distances.

#### 📏 Distance vs. Fare
There’s a strong positive correlation between trip distance and fare amount.

#### 🌤️ Seasonal Variation
Summer months see a notable rise in ride activity, indicating seasonal travel patterns.

#### 🔺 Peak vs. Off-Peak Impact
Average fares are higher during peak hours, likely due to surge pricing and increased demand.

## Conclusion

### Summary of Main Findings
- Peak Ride Times: Uber usage peaks during morning and evening commute hours.

- High-Volume Days: Fridays and Saturdays see the highest number of rides.

- Fare Distribution: Most fares fall between $5 and $20, with some high-value outliers.

- Geographic Concentration: Ride density is greatest in Midtown and Lower Manhattan.

- Airport Trips: These rides typically involve longer distances and higher fares.

- Distance-Fare Relationship: There is a strong positive correlation between fare amount and distance traveled.

- Seasonality: Summer months show a notable increase in ride demand.

- Surge Pricing: Peak-hour rides tend to have slightly elevated fares, indicating demand-based pricing effects.

## Recommendations

### Recommendations: Data-Driven Business Suggestions
#### Optimize Driver Availability During Peak Hours
- Increase driver coverage between 7–9 AM and 5–7 PM to meet demand and reduce wait times.
  
#### Weekend Promotions
- Launch targeted discounts or ride bundles on Fridays and Saturdays to capitalize on high ride volumes.
  
#### Airport Zone Pricing Strategy
- Adjust pricing models or introduce flat-rate fares for airport routes to increase transparency and rider trust.
  
#### Focus on Hotspot Coverage
- Allocate more drivers in Midtown and Lower Manhattan, where demand is consistently high.
  
#### Monitor Seasonal Demand
- Prepare for increased activity during summer months by offering seasonal incentives for drivers.
  
#### Refine Fare Strategy During Off-Peak Times
- Introduce off-peak ride discounts to stimulate usage during lower-demand periods.
  
#### Use Distance-Fare Insights to Improve Estimations
- Enhance fare estimation algorithms using fare-per-km analysis to better inform customers.

## License

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

</body>
</html>
