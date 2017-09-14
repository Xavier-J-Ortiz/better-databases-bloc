1) To find data held in two separate data tables, we could do separate searches on each table, _or_ we could use a `JOIN` statement in order to call different data from different tables into a query.

2) Assuming 2 tables, A and B:

CROSS JOIN - it shows all possible combinations between rows from table A and rows from table B, so a result from the query would be <# of A rows> x <# of B rows>

A real world example would be if I had a table with fruit juices, and another table with entrees at a high school cafeteria. A CROSS JOIN would allow me to compare all the possible combinations of meals for the high schoolers.

INNER JOIN - it's the result of a query between 2 tables where the output is the intersection of table A and table B that satisfies a specified conditional statement between both tables using the `ON` statement.

A real world example would be if, there were a table of students being specified in the `FROM` portion of the query, and another table of different extracurricular societies that identifies the students in the society with their student ID being specified in the `INNER JOIN`, we could get a list of students that are found in a given society using an INNER JOIN between these two tables.

LEFT OUTER JOIN - similar to the INNER JOIN, but assuming table A is specified in the `FROM` statement, in the output query, there would be a row for every row in table A. If the conditional statement specified in `ON` is not met, then this row output null values in the columns of table B.

Similar to the setup of the INNER JOIN, except if we do a LEFT OUTER JOIN instead, we would get the students already in the extracurricular society and also any student that is not in the extracurricular activity. This could be good in order to see which students could be good recruiting targets.

RIGHT OUTER JOIN - similar to LEFT OUTER JOIN, except instead of focusing on the table specified in the `FROM` statement (in the previous case it was table A), we would focus on the table linked to the  `RIGHT OUTER JOIN` statement, in this case, table B. Similarly, there will be a single row in the output for every row in table B, and any row that does not meet the conditional in the `ON` statement will have null values in the output values of columns from table A.

In the extracurricular society example shown before, using a RIGHT OUTER JOIN would yield an output similar to the INNER JOIN output, _plus_ if we've got societies with no members, they would show up. This would be useful for example to pinpoint societies for students interested in building a society from the ground up.

FULL OUTER JOIN - It's the combination of an INNER JOIN, LEFT OUTER JOIN, and RIGHT OUTER JOIN.

Similar to the LEFT OUTER JOIN, RIGHT OUTER JOIN, and the INNER JOIN. We would get the output of the LEFT JOIN, and the RIGHT JOIN and would be useful in being able to easily find the students that have no society, and perhaps also point out to them which societies they would be able to make the most impact in by helping create it from the ground up.

3) *Primary key* would be a key that is a unique identifier in a table, and is only found once within the entire table. This would allow you to identify a single row of data.

A *primary key* real world example would be, for example in college, your student ID #. This would be your primary key in the College's `students` database table.

A *foreign key* would be a key in a table that could represent a *primary key* in another table. Typically used in many to one relationships where we could link multiple of the same *primary keys* in a table by assigning it to a *foreign key* value.

A *foreign key* real world example would be your major's program ID found within your Colleges *student* table. The major ID would be the *primary key* in the College's `program major` table.

4) Aliasing is utilizing a different variable name to represent a table's name or a column' name. Typically done by using the statement `AS` when declaring a `FROM` or `JOIN` statement for  representing a table name, such as:

> SELECT column1 
> FROM table_name AS alias1
> INNER JOIN table2_name
> WHERE alias1.id = 20;

or to represent an aliased column:

> SELECT column1 AS alias1
> FROM table_name
> WHERE alias1 = 20;

5) 
SELECT p.name, c.salary, c.vacation_days 
FROM professor AS p JOIN
compensation AS c ON p.id = c.professor_id;

6)
`NATURAL JOIN` would be ideal to use in cases where column names have the exact same name in both tables. For example, if we have a `fantasy_gamer` table with the the columns `real_name`, `email_address`, and another table called `charachters` with the columns `charachter_name` and `email_address`, we would be able to create the following query:

```
SELECT fantasy_gamer.real_name
FROM fantasy_gamer
NATURAL JOIN charachters;

```

this query would yield the real names of users with emails that match in both `fantasy_gamer` table and the `charachters` table

7) 

I created these tables:
```
contract_business-# ORDER BY shift_id ;

 shift_id | day_of_week | shift | employee_id 
----------+-------------+-------+-------------
        0 | Sunday      |     0 |           3
        1 | Sunday      |     1 |           4
        2 | Sunday      |     2 |           5
        3 | Monday      |     0 |           2
        4 | Monday      |     1 |           0
        5 | Monday      |     2 |           1
        6 | Tuesday     |     0 |           2
        7 | Tuesday     |     1 |           0
        8 | Tuesday     |     2 |           1
        9 | Wednesday   |     0 |           2
       10 | Wednesday   |     1 |           0
       11 | Wednesday   |     2 |           1
       12 | Thursday    |     0 |           2
       13 | Thursday    |     1 |           0
       14 | Thursday    |     2 |           1
       15 | Friday      |     0 |           2
       16 | Friday      |     1 |           0
       17 | Friday      |     2 |           1
       18 | Saturday    |     0 |           3
       19 | Saturday    |     1 |           4
       20 | Saturday    |     2 |           5
(21 rows)
```

and...

```
contract_business=# SELECT * FROM employees;

 employee_id |  name   | gender 
-------------+---------+--------
           0 | Xavier  | M
           1 | Maria   | F
           2 | Roger   | M
           3 | Justine | F
           4 | Omar    | M
           5 | Tammy   | F
```

So for the first part, this is listing all employees and all shifts:

```
SELECT a.name, b.day_of_week, b.shift
FROM employees AS a
INNER JOIN shifts AS b
ON a.employee_id = b.employee_id;
```

OR because I used the same column name `employee_id` in both tables `employees` and `shifts`, I could use `NATURAL JOIN`.

```
contract_business=# SELECT a.name, b.day_of_week, b.shift
FROM employees AS a
NATURAL JOIN shifts AS b;
```
For the second part, I was albe to create a list of possible schedules using `CROSS JOIN`:

```
contract_business=# SELECT a.name, b.day_of_week, b.shift
FROM employees AS a
CROSS JOIN shifts AS b;
```

8) 

SELECT v.name, d.name
FROM volunteer AS v
LEFT OUTER JOIN dogs AS d
ON v.foster_id = d.id;

SELECT a.first_name, a.last_name, d.name
FROM adoptions
WHERE adoptions.date >= '2017-08-11'
JOIN adopters AS a ON a.id = adoptions.adopter
JOIN dog AS d ON d.id = adoptions.dog;

SELECT a.name, d.dogs
FROM adoptions
WHERE adoptions.dog IS NULL
INNER JOIN adopters AS a ON a.id = adoptions.adopter
CROSS JOIN dogs AS d;

SELECT d.name, c.name
FROM adoptions
FULL OUTER JOIN cats AS c
FULL OUTER JOIN dogs AS d
WHERE 
(adoptions.cat IS NULL AND adoptions.dog IS NOT NULL) 
OR 
(adoptions.dog IS NULL AND adoptions.cat IS NOT NULL);

SELECT v.name, d.name
FROM volunteers AS v
FULL OUTER JOIN dogs AS d ON v.foster_id = d.id;

9)

SELECT a.name
FROM adoptions
INNER JOIN cats AS c ON c.id = adoptions.cats
WHERE cats.name = 'Seashell'
INNER JOIN adopters AS a ON a.id = adoptions.adopter;

10)

SELECT p.name h.rank
FROM holds AS h
JOIN patrons AS p ON p.id = h.user_id
JOIN books AS b ON b.isbn = h.isbn
WHERE b.title = 'Harry Potter and the Sourcerer's Stone'
ORDER BY rank ASC;

SELECT b.title t.check_out_date
FROM books AS b
LEFT JOIN transactions AS t ON t.checked_in_date IS NULL;

SELECT AVG(t.checked_in_date - t.checked_out_date) AS read_time, b.title
FROM books AS b
JOIN transactions AS t ON t.checked_out_date >= '2017-08-12'
GROUP BY b.title
ORDER BY read_time ASC;

SELECT b.title
FROM books AS b
LEFT JOIN transactions AS t ON t.checked_out_date >= '2012-09-12'
WHERE t.checked_out_date IS NULL;

SELECT p.name, b.book
FROM patrons AS p
LEFT JOIN transactions AS t ON (p.id = t.id AND t.checked_in_date IS NULL)
LEFT JOIN book AS b ON b.isbn = t.isbn;

