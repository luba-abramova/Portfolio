# SQL tasks
This project is based on resolving tasks provided by SQL Academy, utilizing the MySQL database "Airbnb" as the primary dataset for analysis and exploration.

Data model:
![Data model](/SQL%20challenges/data_model.png)

Tasks:
1. Display housing identifiers and the presence of the Internet in the housing. If there is Internet in the rented housing, then print "YES", otherwise "NO".
```
SELECT id,
	CASE
		WHEN has_internet = 1 THEN 'YES'
		ELSE 'NO'
	END AS has_internet
FROM Rooms;
```
2. Print users who have a Belarusian phone number? The telephone code of Belarus is +375.
```
SELECT *
FROM Users
WHERE phone_number LIKE '+375%';
```

3. Output a list of rooms that have been reserved for at least one day in the 12th week of 2020. In this task, take a period of seven days as one week, the first of which begins on January 1, 2020. For example, the first week of the year is January 1-7, and the third is January 15-21.
```
SELECT Rooms.*
FROM Rooms
	JOIN Reservations on Rooms.id = Reservations.room_id
WHERE WEEK(start_date, 1) = 12
	AND YEAR(start_date) = 2020;
```
4. List the 2nd-level domain names used by users for email in descending order of popularity. The resulting result must be additionally sorted in ascending order of domain names.
```
SELECT SUBSTRING(email, LOCATE('@', email) + 1) AS domain,
	COUNT(*) AS count
FROM Users
GROUP BY domain
ORDER BY count DESC,
	domain;
```
5. Add a 5-star comment on housing located at 11218 Friel Place, New York on behalf of George Clooney
```
INSERT INTO Reviews
SET id = (
		SELECT COUNT(*) + 1
		FROM Reviews AS a
	),
	rating = '5',
	reservation_id = (
		SELECT re.id
		FROM Reservations re
			JOIN Users u on re.user_id = u.id
			JOIN Rooms ro on re.room_id = ro.id
		WHERE u.name = 'George Clooney'
			AND ro.address = '11218, Friel Place, New York'
	);
```
6. Display the number of reservations for each month of each year that had at least 1 reservation. Sort the result in ascending order of reservation date.
```
SELECT YEAR(start_date) as year,
	MONTH(start_date) as month,
	COUNT(id) as amount
FROM Reservations
GROUP BY year,
	month
ORDER BY year,
	month;
```
7. It is necessary to display the rating for rooms that have been rented at least once, as the average of the rating of reviews, rounded down to the integer.
```
SELECT res.room_id,
	FLOOR(AVG(rev.rating)) as rating
FROM Reviews rev
	JOIN Reservations res on rev.reservation_id = res.id
GROUP BY res.room_id;
```

8. Display a list of rooms with all amenities (TV, Internet, kitchen, and air conditioning), as well as the total number of days and the amount for all days of renting each of these rooms. Fields in the resulting table: home_type, address, days, total_fee
