# Bike_Data
## Table of Contents
- [Project Overview](#project-overview)
- [Data Sources](#data-sources)
- [Tools](#tools)
- [Data Cleaning](data-cleaning)
- [Exploratory Data Analysis](exploratory-data-analysis)
- [Data Analysis](data-analysis)
- [Results/Findings](results/findings)
- [Recommendations](recommendations)

Understanding how casual riders and annual members use Cyclistic bikes differently to convert casual riders into annual riders

### Project Overview

Cyclistic is a Chicago-based bike-share company that aims to maximize the number of annual memberships. This data analysis project seeks to provide insight into the difference between annual membership riders and casual riders. Understanding what could influence riders to update to an annual membership is crucial to maximizing marketing strategies that would work to increase membership numbers. 


### Data Sources

Bike Data: The primary datasets used for this analysis are the "divvy-tripdata" files which can be found [here](https://divvy-tripdata.s3.amazonaws.com/index.html). These datasets are separated by months and include the following columns: ride_id, rideabe_type, started_at, ended_at, start_station_name, start_station_id, end_station_name, end_station_id, and member_casual. I chose data for 12 months, from March 2023 to February 2024. 

### Tools

- Excel - Data Cleaning, Pivot Chart 
- SQL Bigquery - Data Analysis
- Tableau - Creating Reports

### Data Cleaning 
In the initial data preparation phase, we performed the following tasks:
1. Data loading and inspection. 
2. Handling missing values.
3. Data cleaning and formatting.
4. Transforming qualitative data to quantitative data
   
### Exploratory Data Analysis
EDA involved exploring the sales data to answer key questions, such as:
- How do annual members and casual riders use Cyclistic bikes differently?
- Why would casual riders buy Cyclistic annual memberships?
- How do days of the week and seasons influence riders preferences? 
- Do bike types affect riders preferences?
- How can Cyclistic use digital media to influence casual riders to become members?

### Data Analysis

The Data analysis was done in Excel and SQL. 

#### Excel
In Excel, I cleaned and transformed the data. First, I ensured there were no duplicates in the dataset. Next, I checked for extreme values to verify their validity. To calculate the ride length for each data point, I subtracted the bike ride end time from the start time. I then converted this number to seconds for a more straightforward analysis. Irrelevant data was removed, such as the longitude and latitude of bike ride stations. Additionally, I converted the day of the ride into numerical values, with Sunday starting at 1, Monday at 2, and so on until Saturday at 7. This quantified the date data. For further analysis, I created Pivot tables for each month, showing the average ride length for casual versus member riders. Using another Pivot table, I determined the average ride length for casual and member riders based on the day of the week. Finally, I calculated the total number of weekly rides for casual and member riders.

### SQL 
I uploaded all 12 months of data in SQL to do exploratory data analysis.

This query combines data from multiple months (March 2023 to February 2024) into a single dataset using UNION ALL. I used a CASE statement to map the numeric day of the week to its corresponding name. Additionally, I employed a subquery to select the day of the week, count occurrences(which I renamed as 'occurrences'), and sum the ride lengths in seconds (renamed as 'total_ride_lenght_seconds'). I repeated this process for all the months and combined the results using UNION ALL into a single dataset. Finally, outside of the subquery, I used GROUP BY to group the results by the day of the week. I also obtained the total number of rides ('Total_rides') by adding up all the 'occurrences' in the subquery using SUM. Additionally, I calculated the total ride length ('Ride_length_seconds') by summing up all of the 'Total_length' values obtained in the subquery. This analysis provided insight into ride patterns, total ride occurrences, and total ride lengths for each day of the week.

``` SQL
SELECT
    CASE
        WHEN day_of_week = 1 THEN 'Sunday'
        WHEN day_of_week = 2 THEN 'Monday'
        WHEN day_of_week = 3 THEN 'Tuesday'
        WHEN day_of_week = 4 THEN 'Wednesday'
        WHEN day_of_week = 5 THEN 'Thursday'
        WHEN day_of_week = 6 THEN 'Friday'
        ELSE 'Saturday'
    END AS Day,
    SUM(occurrences) AS Total_rides,
    SUM(Total_length) AS Ride_length_seconds
FROM (
    SELECT day_of_week, COUNT(*) AS occurrences, SUM(Ride_length_seconds) AS Total_length
    FROM flowing-blade-419819.Bike_Data.April_2023
    GROUP BY 1


    UNION ALL


    SELECT day_of_week, COUNT(*) AS occurrences, SUM(Ride_length_seconds) AS Total_length
    FROM flowing-blade-419819.Bike_Data.May_2023
    GROUP BY 1


    UNION ALL


    SELECT day_of_week, COUNT(*) AS occurrences, SUM(Ride_length_seconds) AS Total_length
    FROM flowing-blade-419819.Bike_Data.March_2023
    GROUP BY 1
   
    UNION ALL


    SELECT day_of_week, COUNT(*) AS occurrences, SUM(Ride_length_seconds) AS Total_length
    FROM flowing-blade-419819.Bike_Data.June_2023
    GROUP BY 1
   
    UNION ALL


    SELECT day_of_week, COUNT(*) AS occurrences, SUM(Ride_length_seconds) AS Total_length
    FROM flowing-blade-419819.Bike_Data.July_2023
    GROUP BY 1
   
    UNION ALL


    SELECT day_of_week, COUNT(*) AS occurrences, SUM(Ride_length_seconds) AS Total_length
    FROM flowing-blade-419819.Bike_Data.August_2023
    GROUP BY 1
   
    UNION ALL


    SELECT day_of_week, COUNT(*) AS occurrences, SUM(Ride_length_seconds) AS Total_length
    FROM flowing-blade-419819.Bike_Data.September_2023
    GROUP BY 1
   
    UNION ALL


    SELECT day_of_week, COUNT(*) AS occurrences, SUM(Ride_length_seconds) AS Total_length
    FROM flowing-blade-419819.Bike_Data.October_2023
    GROUP BY 1
   
    UNION ALL


    SELECT day_of_week, COUNT(*) AS occurrences, SUM(Ride_length_seconds) AS Total_length
    FROM flowing-blade-419819.Bike_Data.November_2023
    GROUP BY 1
   
    UNION ALL


    SELECT day_of_week, COUNT(*) AS occurrences, SUM(Ride_length_seconds) AS Total_length
    FROM flowing-blade-419819.Bike_Data.December_2023
    GROUP BY 1
   
    UNION ALL


    SELECT day_of_week, COUNT(*) AS occurrences, SUM(Ride_length_seconds) AS Total_length
    FROM flowing-blade-419819.Bike_Data.January_2024
 
    GROUP BY 1
   
    UNION ALL


    SELECT day_of_week, COUNT(*) AS occurrences, SUM(Ride_length_seconds) AS Total_length
    FROM flowing-blade-419819.Bike_Data.February_2024
 
    GROUP BY 1
   
    )AS CombinedData
GROUP BY 1
ORDER BY 1;
```
The following SQL query combines data from multiple months (March 2023 to February 2024) into a single dataset using UNION ALL. I was able to calculate various aggregate metrics for each month. For example:
   - 'Ride_length_total' sums up the ride lengths in seconds 
   - 'Average_ride_length' rounds the average ride length in seconds.
   -  'Total_casual' and 'Total_member' count the number of rides per casual and member users, respectively.
   -  I also found the most frequented day of the  week using the MODE function.
   -  Lastly, I obtained the number of rides on electric and classic bikes.
This provided important insight into ride patterns, average ride lengths, user types, and bike types for each month.
     
``` SQL
SELECT EXTRACT(MONTH FROM started_at) as month, SUM(ride_length_seconds) AS ride_length_total, ROUND(AVG(ride_length_seconds)) AS Average_ride_length, SUM(CASE WHEN member_casual = 'member' THEN 1 ELSE 0 END) AS total_casual, SUM(CASE WHEN member_casual = 'casual' THEN 1 ELSE 0 END) AS total_member,
ROUND(AVG(day_of_week)) AS Mode_day,
SUM(CASE WHEN rideable_type = 'electric_bike' THEN 1 ELSE 0 END) AS electric_count, SUM(CASE WHEN rideable_type = 'classic_bike' THEN 1 ELSE 0 END) AS classic_count
FROM flowing-blade-419819.Bike_Data.October_2023
GROUP BY 1
  UNION ALL
SELECT EXTRACT(MONTH FROM started_at) as month, SUM(ride_length_seconds) AS ride_length_total, ROUND(AVG(ride_length_seconds)) AS Average_ride_length, SUM(CASE WHEN member_casual = 'member' THEN 1 ELSE 0 END) AS total_casual, SUM(CASE WHEN member_casual = 'casual' THEN 1 ELSE 0 END) AS total_member,
ROUND(AVG(day_of_week)) AS Mode_day,
SUM(CASE WHEN rideable_type = 'electric_bike' THEN 1 ELSE 0 END) AS electric_count, SUM(CASE WHEN rideable_type = 'classic_bike' THEN 1 ELSE 0 END) AS classic_count
FROM flowing-blade-419819.Bike_Data.September_2023
GROUP BY 1
  UNION ALL
SELECT EXTRACT(MONTH FROM started_at) as month, SUM(ride_length_seconds) AS ride_length_total, ROUND(AVG(ride_length_seconds)) AS Average_ride_length, SUM(CASE WHEN member_casual = 'member' THEN 1 ELSE 0 END) AS total_casual, SUM(CASE WHEN member_casual = 'casual' THEN 1 ELSE 0 END) AS total_member,
ROUND(AVG(day_of_week)) AS Mode_day,
SUM(CASE WHEN rideable_type = 'electric_bike' THEN 1 ELSE 0 END) AS electric_count, SUM(CASE WHEN rideable_type = 'classic_bike' THEN 1 ELSE 0 END) AS classic_count
FROM flowing-blade-419819.Bike_Data.August_2023
GROUP BY 1
UNION ALL
SELECT EXTRACT(MONTH FROM started_at) as month, SUM(ride_length_seconds) AS ride_length_total, ROUND(AVG(ride_length_seconds)) AS Average_ride_length, SUM(CASE WHEN member_casual = 'member' THEN 1 ELSE 0 END) AS total_casual, SUM(CASE WHEN member_casual = 'casual' THEN 1 ELSE 0 END) AS total_member,
ROUND(AVG(day_of_week)) AS Mode_day,
SUM(CASE WHEN rideable_type = 'electric_bike' THEN 1 ELSE 0 END) AS electric_count, SUM(CASE WHEN rideable_type = 'classic_bike' THEN 1 ELSE 0 END) AS classic_count
FROM flowing-blade-419819.Bike_Data.July_2023
GROUP BY 1
UNION ALL
SELECT EXTRACT(MONTH FROM started_at) as month, SUM(ride_length_seconds) AS ride_length_total, ROUND(AVG(ride_length_seconds)) AS Average_ride_length, SUM(CASE WHEN member_casual = 'member' THEN 1 ELSE 0 END) AS total_casual, SUM(CASE WHEN member_casual = 'casual' THEN 1 ELSE 0 END) AS total_member,
ROUND(AVG(day_of_week)) AS Mode_day,
SUM(CASE WHEN rideable_type = 'electric_bike' THEN 1 ELSE 0 END) AS electric_count, SUM(CASE WHEN rideable_type = 'classic_bike' THEN 1 ELSE 0 END) AS classic_count
FROM flowing-blade-419819.Bike_Data.June_2023
GROUP BY 1
UNION ALL
SELECT EXTRACT(MONTH FROM started_at) as month, SUM(ride_length_seconds) AS ride_length_total, ROUND(AVG(ride_length_seconds)) AS Average_ride_length, SUM(CASE WHEN member_casual = 'member' THEN 1 ELSE 0 END) AS total_casual, SUM(CASE WHEN member_casual = 'casual' THEN 1 ELSE 0 END) AS total_member,
ROUND(AVG(day_of_week)) AS Mode_day,
SUM(CASE WHEN rideable_type = 'electric_bike' THEN 1 ELSE 0 END) AS electric_count, SUM(CASE WHEN rideable_type = 'classic_bike' THEN 1 ELSE 0 END) AS classic_count
FROM flowing-blade-419819.Bike_Data.May_2023
GROUP BY 1
UNION ALL
SELECT EXTRACT(MONTH FROM started_at) as month, SUM(ride_length_seconds) AS ride_length_total, ROUND(AVG(ride_length_seconds)) AS Average_ride_length, SUM(CASE WHEN member_casual = 'member' THEN 1 ELSE 0 END) AS total_casual, SUM(CASE WHEN member_casual = 'casual' THEN 1 ELSE 0 END) AS total_member,
ROUND(AVG(day_of_week)) AS Mode_day,
SUM(CASE WHEN rideable_type = 'electric_bike' THEN 1 ELSE 0 END) AS electric_count, SUM(CASE WHEN rideable_type = 'classic_bike' THEN 1 ELSE 0 END) AS classic_count
FROM flowing-blade-419819.Bike_Data.April_2023
GROUP BY 1
UNION ALL
SELECT EXTRACT(MONTH FROM started_at) as month, SUM(ride_length_seconds) AS ride_length_total, ROUND(AVG(ride_length_seconds)) AS Average_ride_length, SUM(CASE WHEN member_casual = 'member' THEN 1 ELSE 0 END) AS total_casual, SUM(CASE WHEN member_casual = 'casual' THEN 1 ELSE 0 END) AS total_member,
ROUND(AVG(day_of_week)) AS Mode_day,
SUM(CASE WHEN rideable_type = 'electric_bike' THEN 1 ELSE 0 END) AS electric_count, SUM(CASE WHEN rideable_type = 'classic_bike' THEN 1 ELSE 0 END) AS classic_count
FROM flowing-blade-419819.Bike_Data.March_2023
GROUP BY 1
UNION ALL
SELECT EXTRACT(MONTH FROM started_at) as month, SUM(ride_length_seconds) AS ride_length_total, ROUND(AVG(ride_length_seconds)) AS Average_ride_length, SUM(CASE WHEN member_casual = 'member' THEN 1 ELSE 0 END) AS total_casual, SUM(CASE WHEN member_casual = 'casual' THEN 1 ELSE 0 END) AS total_member,
ROUND(AVG(day_of_week)) AS Mode_day,
SUM(CASE WHEN rideable_type = 'electric_bike' THEN 1 ELSE 0 END) AS electric_count, SUM(CASE WHEN rideable_type = 'classic_bike' THEN 1 ELSE 0 END) AS classic_count
FROM flowing-blade-419819.Bike_Data.December_2023
GROUP BY 1
UNION ALL
SELECT EXTRACT(MONTH FROM started_at) as month, SUM(ride_length_seconds) AS ride_length_total, ROUND(AVG(ride_length_seconds)) AS Average_ride_length, SUM(CASE WHEN member_casual = 'member' THEN 1 ELSE 0 END) AS total_casual, SUM(CASE WHEN member_casual = 'casual' THEN 1 ELSE 0 END) AS total_member,
ROUND(AVG(day_of_week)) AS Mode_day,
SUM(CASE WHEN rideable_type = 'electric_bike' THEN 1 ELSE 0 END) AS electric_count, SUM(CASE WHEN rideable_type = 'classic_bike' THEN 1 ELSE 0 END) AS classic_count
FROM flowing-blade-419819.Bike_Data.January_2024
GROUP BY 1
UNION ALL
SELECT EXTRACT(MONTH FROM started_at) as month, SUM(ride_length_seconds) AS ride_length_total, ROUND(AVG(ride_length_seconds)) AS Average_ride_length, SUM(CASE WHEN member_casual = 'member' THEN 1 ELSE 0 END) AS total_casual, SUM(CASE WHEN member_casual = 'casual' THEN 1 ELSE 0 END) AS total_member,
ROUND(AVG(day_of_week)) AS Mode_day,
SUM(CASE WHEN rideable_type = 'electric_bike' THEN 1 ELSE 0 END) AS electric_count, SUM(CASE WHEN rideable_type = 'classic_bike' THEN 1 ELSE 0 END) AS classic_count
FROM flowing-blade-419819.Bike_Data.February_2024
GROUP BY 1
UNION ALL
SELECT EXTRACT(MONTH FROM started_at) as month, SUM(ride_length_seconds) AS ride_length_total, ROUND(AVG(ride_length_seconds)) AS Average_ride_length, SUM(CASE WHEN member_casual = 'member' THEN 1 ELSE 0 END) AS total_casual, SUM(CASE WHEN member_casual = 'casual' THEN 1 ELSE 0 END) AS total_member,
ROUND(AVG(day_of_week)) AS Mode_day,
SUM(CASE WHEN rideable_type = 'electric_bike' THEN 1 ELSE 0 END) AS electric_count, SUM(CASE WHEN rideable_type = 'classic_bike' THEN 1 ELSE 0 END) AS classic_count
FROM flowing-blade-419819.Bike_Data.November_2023
GROUP BY 1
ORDER BY month ASC


```
The following query calculates the total ride length in seconds ('Total_ride_length_seconds') for different periods of the day. It categorizes the ride start times into four time-of-day brackets: 'Night', 'Morning',  'Afternoon', and 'Evening'. I used a CASE statement to assign each ride to one of these time brackets based on the hour of the 'started_at' timestamp. I extracted the hour component from the timestamp data by using EXTRACT. Next, I calculated the total ride length for each timestamp using SUM(ride_length_seconds). Since we are working with different data sets, I used UNION ALL to combine the months into a single dataset.

```Sql
SELECT CASE WHEN EXTRACT(HOUR FROM started_at) >= 23 OR EXTRACT(HOUR FROM started_at) <= 4 THEN 'Night' WHEN EXTRACT(HOUR FROM started_at)BETWEEN 5 AND 11 THEN 'Morning' WHEN EXTRACT(HOUR FROM started_at)BETWEEN 12 AND 17 THEN 'Afternoon' WHEN EXTRACT(HOUR FROM started_at) BETWEEN 18 AND 22 THEN 'Evening' END AS Time_d,
SUM(Total_length) AS Total_ride_length_seconds
FROM
(
    SELECT started_at, SUM(ride_length_seconds) AS Total_length
    FROM flowing-blade-419819.Bike_Data.April_2023
    GROUP BY 1


    UNION ALL


    SELECT started_at, SUM(ride_length_seconds) AS Total_length
    FROM flowing-blade-419819.Bike_Data.May_2023
    GROUP BY 1


    UNION ALL


    SELECT started_at, SUM(ride_length_seconds) AS Total_length
    FROM flowing-blade-419819.Bike_Data.March_2023
    GROUP BY 1
   
    UNION ALL


    SELECT started_at, SUM(ride_length_seconds) AS Total_length
    FROM flowing-blade-419819.Bike_Data.June_2023
    GROUP BY 1
   
    UNION ALL


    SELECT started_at, SUM(ride_length_seconds) AS Total_length
    FROM flowing-blade-419819.Bike_Data.July_2023
    GROUP BY 1
   
    UNION ALL


    SELECT started_at, SUM(ride_length_seconds) AS Total_length
    FROM flowing-blade-419819.Bike_Data.August_2023
    GROUP BY 1
   
    UNION ALL


    SELECT started_at, SUM(ride_length_seconds) AS Total_length
    FROM flowing-blade-419819.Bike_Data.September_2023
    GROUP BY 1
   
    UNION ALL


    SELECT started_at, SUM(ride_length_seconds) AS Total_length
    FROM flowing-blade-419819.Bike_Data.October_2023
    GROUP BY 1
   
    UNION ALL


    SELECT started_at, SUM(ride_length_seconds) AS Total_length
    FROM flowing-blade-419819.Bike_Data.November_2023
    GROUP BY 1
   
    UNION ALL


    SELECT started_at, SUM(ride_length_seconds) AS Total_length
    FROM flowing-blade-419819.Bike_Data.December_2023
    GROUP BY 1
   
    UNION ALL


    SELECT started_at, SUM(ride_length_seconds) AS Total_length
    FROM flowing-blade-419819.Bike_Data.January_2024
 
    GROUP BY 1
   
    UNION ALL


    SELECT started_at, SUM(ride_length_seconds) AS Total_length
    FROM flowing-blade-419819.Bike_Data.February_2024
 
    GROUP BY 1
   
    )AS CombinedData
GROUP BY Time_d
ORDER BY Time_d;
```

### Results/Findings

The analysis results are summarized as follows:
1. This scatter plot shows the relationship between total annual members(on the y-axis) and total casual riders (on the x-axis) over 12 months. The positive correlation suggests that as the number of casual riders increases, the total number of annual membership also tends to rise.
   ![Screenshot 2024-06-24 203549](https://github.com/JeimyGar/Bike_Data/assets/168608064/b8f73249-dfd7-4bfd-a00e-5f0c7134dcb7)

2. The "Monthly Ride Length Comparison: Electric vs. Bike Rides" line graph illustrates ride counts(y-axis) across different months (x-axis). There are two lines: the red line represents classic bike rides, and the blue line represents electric bike rides. Notably, both types of rides exhibit increased activity starting in the third month, with peak usage occurring during the summer months. 
   ![Screenshot 2024-06-24 203514](https://github.com/JeimyGar/Bike_Data/assets/168608064/aa6bf6b4-a82d-4df0-8c10-af2d90ae559e)

3. This "Weekly Total Bike Rides" bar graph displays the total ride count (y-axis) across different days of the week (x-axis). The data was aggregated by summarizing ride information for each day over a 12-month period. As we can see, Thursday is the most popular day for rides.
   ![Screenshot 2024-06-24 202619](https://github.com/JeimyGar/Bike_Data/assets/168608064/ddf60be1-0831-435e-9fda-2b5ebcf32eb6)

4. The "Total Bike Rides by Time of Day" graph depicts ride length (in seconds) on the y-axis against different times of day on the x-axis. The data was aggregated by counting ride starting times over 12 months. Notably, the most popular time for rides is in the afternoon, while the least popular time is at night.

    ![Screenshot 2024-06-24 202902](https://github.com/JeimyGar/Bike_Data/assets/168608064/8951d021-2ade-40cd-8ad3-2ea027e6a3ad)

   


### Recommendations 
The results/ findings stand out a few things. Since members and non-members have a positive correlation, what you do for one will most likely impact the other.

- Peak Riding Seasons 
   - Focus on marketing efforts from April to September, as these seem to be the most popular months for riding.
   - Offer special membership discounts during these months.
 - Referral Program
   - Encourage members to refer others.
- Safety Features
   - Since rides are less common at night, promotional material should highlight security cameras, well-lit stations, reflective equipment, and other safety features to encourage evening and nighttime riding.
- Off-Peak Incentives
   - Provide incentives during the months of January to April and September to December, when business is slow, to encourage member sign-ups. These incentives can include guided tours, free rides, or free helmets.
- A/B testing
      - Evaluate different promotional strategies using A/B testing 


