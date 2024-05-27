# DataCamp Projects
Completed projects from DataCamp SQL course to apply my skills in SQL and PostgreSQL and perform a data analysis from start to finish.

### Project 1: Analyzing Students' Mental Health
Explored and analyzed the students data to see how the length of stay impacts the average mental health diagnostic scores of the international students presented in the study.

```
SELECT stay, 
	COUNT(*) as count_int,
	ROUND(AVG(todep), 2) as average_phq,
	ROUND(AVG(tosc), 2) as average_scs,
	ROUND(AVG(toas), 2) as average_as
FROM students
WHERE inter_dom = 'Inter'
GROUP BY stay
ORDER BY stay DESC;
```

### Analyze International Debt Statistics
Analyzed international debt data collected by The World Bank. The dataset contained information about the amount of debt (in USD) owed by developing countries across several categories. 

What is the number of distinct countries present in the database?

```
SELECT COUNT(DISTINCT country_name) as total_distinct_countries
FROM international_debt;
```

What country has the highest amount of debt?

```
SELECT country_name, SUM(debt) as total_debt
FROM public.international_debt
GROUP BY country_name 
ORDER BY total_debt DESC
LIMIT 1;
```

What country has the lowest amount of repayments?
```
SELECT country_name, indicator_name, MIN(debt) as lowest_repayment
FROM public.international_debt
WHERE indicator_code = 'DT.AMT.DLXF.CD'
GROUP BY country_name, indicator_name
ORDER BY lowest_repayment
LIMIT 1;
```

### Analyzing Electric Vehicle Charging Habits
Analyzed a dataset to help apartment building managers better understand their tenants’ EV charging habits.

Found the number of unique individuals that used each garage’s shared charging stations.

```
SELECT garage_id, 
    COUNT(DISTINCT user_id) as unique_users
FROM public.charging_sessions
WHERE user_type = 'Shared'
GROUP BY public.charging_sessions.garage_id
ORDER BY unique_users DESC;
```

Found the top 10 most popular charging start times (by weekday and start hour) for sessions that used shared charging stations.

```
SELECT weekdays_plugin, start_plugin_hour, 
    COUNT(*) as num_charges
FROM public.charging_sessions
WHERE user_type = 'Shared'
GROUP BY weekdays_plugin, start_plugin_hour 
ORDER BY num_charges DESC
LIMIT 10;
```

Found the users whose average charging sessions lasted longer than 10 hours when using shared charging stations.

```
SELECT user_id, 
    AVG(duration_hours) as avg_charging_duration
FROM public.charging_sessions
WHERE user_type = 'Shared'
GROUP BY user_id
HAVING AVG(duration_hours) > 10
ORDER BY avg_charging_duration DESC;
```

### When Was the Golden Era of Video Games?

Analyzed video game critic and user scores, in addition to sales data for the top 400 video games released since 1977. Searched for a golden age of video games by identifying release years that users and critics liked best. Explored the business side of gaming by looking at game sales data.

```
-- 10_best_selling_games

SELECT *
FROM game_sales
ORDER BY games_sold DESC
LIMIT 10;
```
```
-- critics_top_ten_years

SELECT g.year, u.num_games, 
	ROUND(AVG(r.critic_score), 2) as avg_critic_score
FROM game_sales as g
JOIN reviews r ON g.name = r.name
JOIN users_avg_year_rating as u ON g.year = u.year
WHERE u.num_games >= 4
GROUP BY g.year, u.num_games
ORDER BY avg_critic_score DESC
LIMIT 10;
```
```
-- golden_years

SELECT u.year, u.num_games, 
	c.avg_critic_score, u.avg_user_score, 
	(c.avg_critic_score - u.avg_user_score) as diff
FROM users_avg_year_rating as u
JOIN critics_avg_year_rating as c ON u.year = c.year
WHERE u.avg_user_score > 9 OR c.avg_critic_score > 9
GROUP BY u.year, c.avg_critic_score
ORDER BY u.year;
```