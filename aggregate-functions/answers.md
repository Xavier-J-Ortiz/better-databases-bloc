1)

AVG - computes the average of a given column of selected data.

SUM - computes the sum of a given column of selected data

COUNT - counts the amount of rows within a selected column of data.

MIN - selects the minimum data value within a given column

MAX - selects the maximum data value within a given column.

2)

SELECT SUM(amount)
FROM donations;

SELECT SUM(amount), donor
FROM donations
GROUP BY donor;

SELECT AVG(amount), donor
FROM donations
GROUP BY donor;

SELECT MAX(amount), donor
FROM donations;

SELECT COUNT(amount)
FROM donations
WHERE amount > 100;

SELECT MAX(amount), donor
FROM donations;

SELECT MIN(amount), donor
FROM donations;

3) By using `ORDER BY`

4) A real world situation where I would use `OFFSET` would be during a fund raising competition. I could use `ORDER BY` to organize from the highest funderasier to the lowest, and then use `LIMIT 2 OFFSET 1` to get the second and third highest fundraisers.

5) If you are limiting your results to get meaningful information from the way a query is ordered, it would be important to use `ORDER BY`. For example, the query is returing your top sales performers, and you'd like to limit it to your top 5 performers, it would be important to use `ORDER BY` in order to retrieve these top performers. 

6) `WHERE` followed by a condition is used to filter rows before aggregated by `GROUP BY`. `HAVING` filters after aggregation is performed, and can have conditionals using the aggregated values.

7)

SELECT id, SUM(amount)
FROM payment
GROUP BY id
HAVING SUM(amount) > 200;

8)
SELECT intake_date, name 
FROM cats
ORDER BY intake_date;

SELECT name, date
FROM adoption
ORDER BY date DESC
LIMIT 5;

SELECT name, gender, age
FROM cats
WHERE gender="F"
ORDER BY age DESC
LIMIT 1;


SELECT id, SUM(amount)
FROM donations
GROUP BY id
ORDER BY SUM(amount) DESC
LIMIT 5;

SELECT id, SUM(amount)
FROM donations
GROUP BY id
ORDER BY SUM(amount) DESC
LIMIT 10 OFFSET 5;
