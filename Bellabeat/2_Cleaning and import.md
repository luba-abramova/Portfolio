# Data cleaning and import
## Data cleaning in Excel
I started my analysis with the preliminary data cleaning process in Excel:
* Checked for duplicates: no duplicates were found within the datasets.
* Verified for missing values: there were no instances of missing values detected.
* Examined for outliers: no outliers were identified in the datasets.
* Adjusted data formats: ensured proper formatting for numerical values and dates.
* Improved column names: renamed columns for easier interpretation of data.
* Updated file names: renamed files for better organization and accessibility during analysis.

## Import data in MS SQL
I've imported 4 tables to MS SQL using Import Flat File Wizard. Checked the data consistency:
```
SELECT COUNT(distinct user_id) 
FROM daily_activity_03_04; -- 35 distinct users

SELECT COUNT(distinct user_id) 
FROM hourly_steps_03_04; -- 34 distinct users

SELECT COUNT(distinct user_id) 
FROM daily_activity_04_05; --33 distinct users

SELECT COUNT(distinct user_id) 
FROM hourly_steps_04_05; -- 33 distinct users
```

The number of distinct users differs between datasets, which is expected and acceptable. The dataset does not represent the continuous activity of the same 30 users throughout the entire period. Upon examining the data across all tables, I observed additional user IDs present in each dataset. 

For example:
```
SELECT DISTINCT user_id
FROM hourly_steps_03_04
EXCEPT 
SELECT DISTINCT user_id 
FROM hourly_steps_04_05;

-- 2 unique users found

SELECT DISTINCT user_id
FROM hourly_steps_04_05
EXCEPT 
SELECT DISTINCT user_id 
FROM hourly_steps_03_04;

-- 1 unique user found
```

After that, I conbined daily_activity_03_04 and daily_activity_04_05 into table daily_activity: 
```
SELECT * INTO daily_activity
FROM daily_activity_03_04 
UNION ALL
SELECT * 
FROM daily_activity_04_05;
```
And hourly_steps_03_04 and hourly_steps_04_05 tables into hourly_steps for further analysis and visuzlization:
```
SELECT * INTO hourly_steps
FROM hourly_steps_03_04
UNION ALL
SELECT * 
FROM hourly_steps_04_05;
```