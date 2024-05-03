# Data import and combining

1. Imported 12 CSV files into SQL Server.

![](/Cyclistic/screenshots/import_MSSQL.png)

2. Combined data from 12 months into one table "combined_tripdata_2023":
```
SELECT * INTO combined_tripdata_2023 
FROM tripdata_01_2023 UNION  
SELECT * FROM tripdata_02_2023 UNION
SELECT * FROM tripdata_03_2023 UNION
SELECT * FROM tripdata_04_2023 UNION
SELECT * FROM tripdata_05_2023 UNION
SELECT * FROM tripdata_06_2023 UNION
SELECT * FROM tripdata_07_2023 UNION
SELECT * FROM tripdata_08_2023 UNION
SELECT * FROM tripdata_09_2023 UNION
SELECT * FROM tripdata_10_2023 UNION
SELECT * FROM tripdata_11_2023 UNION
SELECT * FROM tripdata_12_2023;
```
Checked the completeness of the data, whether any data was lost during import:
```
SELECT COUNT(*)
FROM combined_tripdata_2023;
```
the result: no data was lost :)
![](/Cyclistic/screenshots/import_count.png)

# Data cleaning (SQL)
1.	It's time to decide what to do with NULL values.

How many NULL values were there?
```
SELECT 
  COUNT (*) - COUNT(rideable_type) as rideable_type,
  COUNT (*) - COUNT(started_at) as started_at,
  COUNT (*) - COUNT(ended_at) as ended_at,
  COUNT (*) - COUNT(start_station_name) as start_station_name,
  COUNT (*) - COUNT(start_station_id) as start_station_id,
  COUNT (*) - COUNT(end_station_name) as end_station_name,
  COUNT (*) - COUNT(end_station_id) as end_station_id,
  COUNT (*) - COUNT(start_lat) as start_lat,
  COUNT (*) - COUNT(start_lng) as start_lng,
  COUNT (*) - COUNT(end_lat) as end_lat,
  COUNT (*) - COUNT(end_lng) as end_lng,
  COUNT (*) - COUNT(member_casual) as member_casual
FROM combined_tripdata_2023;
```
If the percentage of missing data was insignificant (less than 5%), we could proceed with our analysis by excluding rows with NULL values. This approach was unlikely to significantly impact the comprehensiveness of our analysis. However, if the percentage of missing data was higher, omitting these rows could compromise the representativeness of our sample and introduce bias into the analysis results.

|column|number of NULL|% of total|
|---|---|---|
|start_station_name|875,716|15,31|
|start_station_id|875,848|15,31|
|end_station_name|929,202|16,25|
|end_station_id|929,343|16,25|
|end_lat|6,990|0,12|
|end_lng|6,993|0,12|

Deleting 15-16% of data will affect the sample size and the final result may be biased, so we will leave these values for further analysis. The missing data mostly comes from start and end stations information, so it won’t affect the main analysis.

2. After that, I updated the combined table by converting the day_of_week column from a numeric value to a text value. This conversion enhanced the visualization and facilitated a clearer understanding of the analysis.

```
ALTER TABLE combined_tripdata_2023
ALTER COLUMN day_of_week varchar(10);

UPDATE combined_tripdata_2023
SET day_of_week = CASE
					WHEN day_of_week = '1' THEN 'Monday'
					WHEN day_of_week = '2' THEN 'Tuesday'
					WHEN day_of_week = '3' THEN 'Wednesday'
					WHEN day_of_week = '4' THEN 'Thursday'
					WHEN day_of_week = '5' THEN 'Friday'
					WHEN day_of_week = '6' THEN 'Saturday'
					ELSE 'Sunday'
				  END;
```
Finally, our data got ready for analysis.