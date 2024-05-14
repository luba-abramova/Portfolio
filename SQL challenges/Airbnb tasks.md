# SQL tasks - Airbnb
This project is based on resolving tasks provided by [SQL Academy](https://sql-academy.org/en), utilizing the MySQL database "Airbnb" as the primary dataset for analysis and exploration.

Data model:

![Data model](/SQL%20challenges/data_model.png)

Tasks:
1. Display housing identifiers and the presence of the Internet in the housing. If there is Internet in the rented housing, then print "YES", otherwise "NO".
```
SELECT id,
	CASE WHEN has_internet = 1 THEN 'YES'
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
WHERE WEEK(start_date, 1) = 12 AND YEAR(start_date) = 2020;
```
4. List the 2nd-level domain names used by users for email in descending order of popularity. The resulting result must be additionally sorted in ascending order of domain names.
```
SELECT SUBSTRING(email, LOCATE('@', email) + 1) AS domain,
	COUNT(*) AS count
FROM Users
GROUP BY domain
ORDER BY count DESC, domain;
```
5. Add a 5-star comment on housing located at 11218 Friel Place, New York on behalf of George Clooney
```
INSERT INTO Reviews
SET id = (SELECT COUNT(*) + 1
		FROM Reviews AS a),
	rating = '5',
	reservation_id = (SELECT re.id
		FROM Reservations re
		JOIN Users u on re.user_id = u.id
		JOIN Rooms ro on re.room_id = ro.id
		WHERE u.name = 'George Clooney'
		AND ro.address = '11218, Friel Place, New York');
```
6. Display the number of reservations for each month of each year that had at least 1 reservation. Sort the result in ascending order of reservation date.
```
SELECT YEAR(start_date) as year,
	MONTH(start_date) as month,
	COUNT(id) as amount
FROM Reservations
GROUP BY year, month
ORDER BY year, month;
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
```
SELECT home_type, address,
	IFNULL(SUM(TIMESTAMPDIFF(DAY, start_date, end_date)), 0) as days,
	IFNULL(SUM(total), 0) as total_fee
FROM Rooms r
LEFT JOIN Reservations re on r.id = re.room_id
WHERE has_air_con = 1
AND has_internet = 1
AND has_kitchen = 1
AND has_tv = 1
GROUP BY home_type, address;
```
9. It is necessary to categorize rooms into economy, comfort, premium at a price <= 100, 100 < price < 200, >= 200, respectively. As a result, display a table with the name of the category and the number of housing falling into this category.
```
SELECT 
	CASE WHEN price <= 100 THEN 'economy'
	WHEN price > 100 AND price < 200 THEN 'comfort'
	WHEN price >= 200 THEN 'premium'
	END as category,
	COUNT(*) as count
FROM Rooms
GROUP BY category;
```
10. Print the average booking price per day for each of the rooms that have been booked at least once. The average cost must be rounded up to an integer value.
```
SELECT room_id,
	CEILING(AVG(price)) as avg_price
FROM Reservations
GROUP BY room_id;
```
11. Print the id of those rooms that have been rented an odd number of times.
```
SELECT room_id,
	COUNT(*) as count
FROM Reservations
GROUP BY room_id
HAVING MOD(count, 2) != 0;
```
12. Display the names of all users of the booking service, as well as the status of whether the user is the owner of the housing and whether he has rented the housing at least once. If the user has the specified status, display 1 in the appropriate field, otherwise 0.
```
SELECT name,
	IF(id IN (SELECT owner_id FROM Rooms), 1, 0) as is_owner,
	IF(id IN (SELECT user_id FROM Reservations), 1, 0) as is_tenant
FROM Users;
```
13. Display all users with email in "hotmail.com". Fields in the resulting table: *
```
SELECT *
FROM Users
WHERE email LIKE '%@hotmail.com';
```
14. Output the fields id, home_type, price for all residential units from the Rooms table. If the room has a TV and Internet at the same time, then display the price in the price field, applying a 10% discount.
```
SELECT id, home_type,
	IF(has_tv AND has_internet, price * 0.9, price) as price
FROM Rooms;
```
15. Create a "Verified_Users" view with id, name and email fields that will only show users who have a verified email address.
```
CREATE VIEW Verified_Users AS
SELECT id, name, email
FROM Users
WHERE email_verified_at IS NOT NULL;
```
16. For each room selected at least 1 time, find the name of the person who last rented it and used it when they checked out.
```
WITH max_date as(
	SELECT room_id, MAX(end_date) as end_date
	FROM Reservations
	GROUP BY room_id)
SELECT r.room_id, u.name, r.end_date
FROM max_date m
JOIN Reservations r on m.room_id = r.room_id
AND m.end_date = r.end_date
JOIN Users u on r.user_id = u.id;
```
17. Display the identifiers of all the owners of the rooms that are placed on our service for choosing accommodation and the value they have earned.
```
SELECT r.owner_id,
	IFNULL(SUM(res.total), 0) as total_earn
FROM Rooms r
LEFT JOIN Reservations res on r.id = res.room_id
GROUP BY r.owner_id;
```
18. Find what percentage of users registered on the booking service have rented or rented out a property at least once. Round the result to the nearest hundredth.
```
SELECT ROUND(
		(SELECT COUNT(*)
		FROM (SELECT DISTINCT owner_id
		FROM Rooms r
		JOIN Reservations res ON r.id = res.room_id
		UNION
		SELECT user_id
		FROM Reservations) as users) * 100 / 
		(SELECT COUNT(*)
		FROM Users), 
	2) AS percent;
```