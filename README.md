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
- [Limitations](limitations)
- [References](references)
  

Understanding how casual riders and annual members use Cyclistic bikes differently to convert casual riders into annual riders

### Project Overview

Cyclistic is a bike_share company based in Chicago that wants to maximize the number of annual memberships. This data analysis project aims to provide insight into how annual members and casual riders differ. This is important to understand why casual riders would buy a membership and how digital media could affect their marketing tactics.


### Data Sources

Bike Data: The primary datasets used for this analysis are the "Index of Bucket" files which can be found HERE. These datasets are seperated by months. They include the following columns: ride_id, rideabe_type, started_at, ended_at, start_station_name, start_station_id, end_station_name, end_station_id, and member_casual. I chose datasets for 12 months starting in March 2023 to February 2024. 

### Tools

- Excel - Data Cleaning, Pivot Chart 
- SQL Bigquery - Data Analysis
- Tableau - Creating Reports

### Data Cleaning 
In the initial data preparation phase, we performed the following tasks:
1. Data loading and inspection.
2. Handling missing values.
3. Data cleaning and formatting. 

### Exploratory Data Analysis
EDA involved exploring the sales data tp answer key questions, such as:
- How do annual members and casual riders use Cyclistic bikes differently?
- Why would casual riders buy Cyclistic annual memberships?
- How do days of the week and seasons influence riders prefrences? 
- Do bike types affect riders prefrences?
- How can Cyclistic use digital media to influence casual riders to become members?

### Data Analysis

The Data analysis was mostly done on SQL 

This first SQL querie was done to try and add up the total rides and the ride length for each day of the week for all 12 months. CASE WHEN was used to convert numerical day of the week into its actual day. A subquery was used to sum up the total rides per day for each month UNION ALL was used to combine the results of each month from March 2023 to February 2024
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
This SQL code was used to join the collective 12 month data and determine the use patterns for bikes throught the day
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
1. customer

### Recommendations 
What actions would i recommend the company to take? use bullet points. Can say something like "Invest in marketing and promotions during peak sales season to maximze revenue"


### Limitations 
Did you need to modify the data in anyway?

### References

1. Links to code that you used or book you used or anything like that
2. 2


