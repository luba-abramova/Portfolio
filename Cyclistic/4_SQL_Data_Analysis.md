# Data analysis with SQL and MS SQL
At this stage, I conducted a detailed analysis to explore the differences between annual members and casual riders, aiming to identify trends and relationships within the data. 

List of queries:

1. Total number of member and casual rides:
```
SELECT member_casual, COUNT(*) AS total_trips
FROM combined_tripdata_2023
GROUP BY member_casual;
```
2. Bikes that members and casual riders choose, and % of total rides:
```
SELECT member_casual, rideable_type, 
    COUNT(*) AS num_of_trips, 
    ROUND(
    CAST(COUNT(*) * 100 as float) / 
        CAST(SUM(COUNT(*)) over() as float), 
    2) AS percent_of_total
FROM combined_tripdata_2023
GROUP BY member_casual, rideable_type
ORDER BY num_of_trips DESC;
```
3. Hours of day summary, member and casual total rides:
```
SELECT member_casual, 
    DATEPART(hour, started_at) AS hour, 
    COUNT(*) AS num_trips
FROM combined_tripdata_2023
GROUP BY member_casual, DATEPART(hour, started_at)
ORDER BY num_trips DESC;
```
4. Days of week summary, member and casual total rides:
```
SELECT member_casual, day_of_week, 
    COUNT(*) AS num_trips
FROM combined_tripdata_2023
GROUP BY member_casual, day_of_week
ORDER BY num_trips DESC;
```
5. Months of year summary, member and casual total rides:
```
SELECT member_casual, 
    DATENAME(month, started_at) AS month, 
    COUNT(*) AS num_trips
FROM combined_tripdata_2023
GROUP BY member_casual, DATENAME(month, started_at)
ORDER BY num_trips DESC;
```
6. Average ride length for member and casual rides:
```
SELECT member_casual, 
    ROUND(
    AVG(DATEDIFF(SECOND, started_at, ended_at) / 60), 
    2) AS avg_minutes_ride_length
FROM combined_tripdata_2023
GROUP BY member_casual;
```
7. Average ride length by day of a week:
```
SELECT member_casual, day_of_week, 
    AVG(
    ROUND(
        DATEDIFF(SECOND, started_at, ended_at) / 60,
        1)) AS avg_minutes_ride_length, 
    COUNT(*) AS num_trips
FROM combined_tripdata_2023
GROUP BY member_casual, day_of_week
ORDER BY avg_minutes_ride_length DESC;
```
8. Average ride length by month:
```
SELECT member_casual, 
    DATENAME(month, started_at) AS month, 
    AVG(
    ROUND(
        DATEDIFF(SECOND, started_at, ended_at) / 60, 
        1)) AS avg_minutes_ride_length, 
    COUNT(*) AS num_trips
FROM combined_tripdata_2023
GROUP BY member_casual, DATENAME(month, started_at)
ORDER BY avg_minutes_ride_length DESC;
```
9. Mode day of week - busiest day among member and casual rides:
```
WITH mode_day_of_week AS(
    SELECT TOP 2
    member_casual,
    day_of_week,
    ROW_NUMBER() OVER (
        PARTITION BY member_casual 
        ORDER BY COUNT(day_of_week) DESC
        ) AS row
    FROM combined_tripdata_2023
    GROUP BY member_casual, day_of_week
    ORDER BY row
    )
SELECT member_casual, day_of_week
FROM mode_day_of_week;
```
10. Average distance per months for member and casual rides:
 ```
SELECT member_casual, DATENAME(month, started_at) as month,
    ROUND(
    AVG(
        geography::Point(start_lat, start_lng, 4326)
        .STDistance(geography::Point(end_lat, end_lng,4326))
        ),
    1) AS distance
FROM combined_tripdata_2023
WHERE start_lat IS NOT NULL AND start_lng IS NOT NULL AND end_lat IS NOT NULL AND end_lng IS NOT NULL
GROUP BY member_casual, DATENAME(month, started_at);
```
11. 50 most popular start stations among members:
```
SELECT TOP 50 
    start_station_name, start_lat, start_lng,
    SUM(
        CASE WHEN member_casual = 'member' THEN 1 ELSE 0 
        END) AS members
FROM combined_tripdata_2023
WHERE start_station_name IS NOT NULL
GROUP BY start_station_name, start_lat, start_lng
ORDER BY members DESC;
```
12. ...and casual riders:
```
SELECT TOP 50 
    start_station_name, start_lat, start_lng,
    SUM(
        CASE WHEN member_casual = 'casual' THEN 1 ELSE 0 
        END) AS casual
FROM combined_tripdata_2023
WHERE start_station_name IS NOT NULL
GROUP BY start_station_name, start_lat, start_lng
ORDER BY casual DESC;
```
13. 50 most popular end stations among members:
```
SELECT TOP 50 
    end_station_name, end_lat, end_lng,
    SUM(
        CASE WHEN member_casual = 'member' THEN 1 ELSE 0 
        END) AS members
FROM combined_tripdata_2023
WHERE end_station_name IS NOT NULL
GROUP BY end_station_name, end_lat, end_lng
ORDER BY members DESC;
```
14. ...and casual riders:
```
SELECT TOP 50 
    end_station_name, end_lat, end_lng,
    SUM(
        CASE WHEN member_casual = 'casual' THEN 1 ELSE 0 
        END) AS casual
FROM combined_tripdata_2023
WHERE end_station_name IS NOT NULL
GROUP BY end_station_name, end_lat, end_lng
ORDER BY casual DESC;
```

After executing all queries, I have saved result tables to .xlsx files and continued with data visualization.






