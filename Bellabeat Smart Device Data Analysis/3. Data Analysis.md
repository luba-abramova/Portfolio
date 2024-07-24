# Data analysis
## Audience segmentation
For marketing analysis, I decided to segment our audience. 

During the analysis process, I identified that some users had multiple records within a single day, indicating that they wore and removed the tracker multiple times. However, for our purposes, we only required information about daily usage, regardless of how many times a user wore the tracker.

To segment our audience, I categorized users into active, moderate and passive based on their level of engagement with our products. These segments were defined by calculating the mean (or average) and standard deviation, which was determined to be 9.35 (calculated in Excel and the attached report as a PNG file to the folder).


On average, a person used the device for 39 days out of 62.
```
SELECT COUNT(*) / COUNT(DISTINCT user_id)
FROM daily_activity;
```
| |(No column name)|
|---|---|
|1|39|

After that, I counted the number of active days for each user:
```
SELECT user_id, COUNT(DISTINCT date) AS active_days
FROM daily_activity
GROUP BY user_id;
```

Next, I assigned individuals to segments:
```
WITH new_daily_activity AS
	(SELECT user_id,
		CASE WHEN COUNT(*) < 30 THEN 'passive'
		WHEN COUNT(*) >= 30 AND COUNT(*) < 49 THEN 'moderate'
		ELSE 'active'
		END AS user_category
	FROM daily_activity
	GROUP BY user_id)
SELECT a.*, user_category
FROM daily_activity a
JOIN new_daily_activity b on a.user_id = b.user_id;
```
And saved the table in .xlsx format to use it in Tableau. 

Additionally, for the same purpose, I added this segmentation to "hourly_steps" table and saved it accordingly:

```
WITH new_hourly_steps AS
	(SELECT user_id,
		CASE WHEN COUNT(*) < 30 THEN 'passive'
		WHEN COUNT(*) >= 30 AND COUNT(*) < 49 THEN 'moderate'
		ELSE 'active'
		END AS user_category
	FROM daily_activity
	GROUP BY user_id) 
SELECT a.*, user_category
FROM hourly_steps a
JOIN new_hourly_steps b on a.user_id = b.user_id;
```
With these new saved tables, I proceeded with the analysis in Tableau to explore and interpret data.
