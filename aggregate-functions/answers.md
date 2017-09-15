1)

AVG - computes the average of a given column of selected data.

average class score:

```
SELECT AVG(score)
FROM students;
```

SUM - computes the sum of a given column of selected data

Total donations:

```
SELECT SUM(donation)
FROM contributors;
```

COUNT - counts the amount of rows within a selected column of data.

Amount of students in a class:

```
SELECT COUNT(*)
FROM algebra_2017;
```

MIN - selects the minimum data value within a given column

Height of the shortest man on the planet

```
SELECT MIN(height)
FROM world_medical_records;
```

MAX - selects the maximum data value within a given column.

```
SELECT MAX(height)
FROM world_medical_records;
```
2)

SELECT SUM(amount)
FROM donations
WHERE date >= 2016-09-14;

SELECT SUM(amount), donor
FROM donations
GROUP BY donor;

SELECT AVG(amount), donor
FROM donations
GROUP BY donor;

SELECT COUNT(amount)
FROM donations
WHERE amount > 100;

SELECT MAX(amount), donor
FROM donations
GROUP BY donor
ORDER BY 1 DESC
LIMIT 1;

SELECT MIN(amount), donor
FROM donations
GROUP BY donor
ORDER BY 1 ASC
LIMIT 1;

3) By using `ORDER BY`. `ORDER BY` allows you to order the output of your query by using a column of that query to sort the query's presentation. Also, after choosing the columns by which you want to order the query by, you can specify if you want it sorted in ascending order, or decending order.

So, an `ORDER BY` clause can look like this:

```
SELECT name, height
FROM medical_records
ORDER BY height DESC;
```

The table would order the query set by height from highest to lowest.

4) A real world situation where I would use `OFFSET` would be during a fund raising competition. Lets say that the winner is known, and I want to find the second and third place winner I could use `ORDER BY` to only show the second and third highest fundraisers. I would use  `LIMIT 2 OFFSET 1` to get the second and third highest fundraisers like so:

```
SELECT donor, amount
FROM donations
ORDER BY amount DESC
LIMIT 2 OFFSET 1;
```

5) If you are limiting your results to get meaningful information from the way a query is ordered, it would be important to use `ORDER BY`. For example, the query is returing your top sales performers, and you'd like to limit it to your top 5 performers, it would be important to use `ORDER BY` in order to retrieve these top performers. 

If you use `LIMIT` without using `ORDER BY` you may get partial information from the whole dataset. Because of this you may be missing relevant information, given that the LIMITed dataset will present data that might not be from a set that would be useful for you.

For example if you have a table of 20 professional athletes, and want to get the gold, silver, and bronze medalist winners by using `LIMIT 3`, and do not specify `ORDER BY DESC` the 3 resulting athletes may or may not be the 3 placing athletes in the right place order. This is because the LIMITed 3 athletes that are the result of the query will most likely be athletes that happened to be in the first, second, or third row of the query.

6) `WHERE` followed by a condition is used to filter rows before aggregated by `GROUP BY`. What this means is that before SQL aggregation happens, i.e. SUM, AVG, MIN, the dataset is filtered with the logical condition specified within the `WHERE`. Only after this `WHERE` condition filters the dataset, does the actual aggregation (SUM, AVG, MIN) occurr on this filtered dataset. 

For example, if you have a scores column, and you want to do AVG(scores), and because you're a benevolent grader, you'd want to remove all `scores = 0`, you can use the `WHERE` clause to only include `score != 0`, and _then_ do the aggregation. This will basically forgive any student with a score of 0 on a test.

The query would look something like this:

```
SELECT AVG(score), student_name
FROM scores
WHERE score != 0
GROUP BY student_name;
```

`HAVING` filters after aggregation is performed, and can have conditionals using the aggregated values. In this case, once the aggregation is performed on the dataset, `HAVING` apply the logical condition to the dataset.

So, following the scenario described above, say you're grading and you've had a bad day. This would be the best time to create a list of all students who fail your class this semester! In your dataset, you would use AVG(score). It would filter the dataset resulting after the data is aggregated, `HAVING` would then apply a logical condition to the dataset. Using `HAVING AVG(scores) < 70` would then only display students who's yearly score would be less than 70%. The list would then include students that could easily be shamed. 

```
SELECT AVG(score), student_name
FROM scores
GROUP BY student_name
HAVING AVG(score) < 70;
```

In terms of performance, I've read in an [article](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=1&cad=rja&uact=8&ved=0ahUKEwi_s9bVlabWAhVl64MKHS6jCXYQFggmMAA&url=https%3A%2F%2Fstackoverflow.com%2Fquestions%2F328636%2Fwhich-sql-statement-is-faster-having-vs-where&usg=AFQjCNFgP3uB-LVc2FTHe_Y0v3MM3b66kQ) it's better to use a the `WHERE` theoretically as it would yield a result set before returning rows, `HAVING` would restrict the result set after bringing all the rows, implying that because of this `WHERE` is faster.

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
FROM adoptions
ORDER BY date DESC
LIMIT 5;

SELECT name, gender, age
FROM cats
WHERE gender = 'F' AND age > 2
ORDER BY age DESC;

SELECT donor, SUM(amount)
FROM donations
GROUP BY donor
ORDER BY SUM(amount) DESC
LIMIT 5;

SELECT donor, SUM(amount)
FROM donations
GROUP BY donor
ORDER BY SUM(amount) DESC
LIMIT 10 OFFSET 5;
