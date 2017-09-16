1) To find data held in two separate data tables, we could do separate searches on each table, _or_ we could use a `JOIN` statement in order to call different data from different tables into a query.

2) Assuming 2 tables, A and B:

**CROSS JOIN** - it shows all possible combinations between rows from table A and rows from table B, so a result from the query would be <# of A rows> x <# of B rows>

A real world example would be if I had a table with fruit juices, and another table with entrees at a high school cafeteria. A CROSS JOIN would allow me to compare all the possible combinations of meals for the high schoolers.

```
SELECT m.dish, d.drink
FROM meals AS m
CROSS JOIN drink AS d;
```

**INNER JOIN** - it's the result of a query between 2 tables where the output is the intersection of table A and table B that satisfies a specified conditional statement between both tables using the `ON` statement.

A real world example would be if, there were a table of students being specified in the `FROM` portion of the query, and another table of different extracurricular societies that keeps records of students by their student ID. We could use the `INNER JOIN`, to get a list of students that are found in a given society using an INNER JOIN between these two tables. The `INNER JOIN` would return matches as specified in the `ON` statement. The query would look something like this.

```
SELECT students.name societies.society
FROM students
INNER JOIN societies ON students.id = societies.student_id;
```

**LEFT OUTER JOIN**- similar to the INNER JOIN, but assuming table A is specified in the `FROM` statement, in the output query, there would be a row for every row in table A. If the conditional statement specified in `ON` is not met, then this row output null values in the columns of table B.

Similar to the setup of the INNER JOIN, except if we do a LEFT OUTER JOIN instead, we would get the students already in the extracurricular society and also any student that is not in the extracurricular activity. This could be good in order to see which students could be good recruiting targets.

```
SELECT students.name societies.society
FROM students
LEFT JOIN societies ON students.id = societies.student_id;
```

**RIGHT OUTER JOIN** - similar to LEFT OUTER JOIN, except instead of focusing on the table specified in the `FROM` statement (in the previous case it was table A), we would focus on the table linked to the  `RIGHT OUTER JOIN` statement, in this case, table B. Similarly, there will be a single row in the output for every row in table B, and any row that does not meet the conditional in the `ON` statement will have null values in the output values of columns from table A.

In the extracurricular society example shown before, using a RIGHT OUTER JOIN would yield an output similar to the INNER JOIN output, _plus_ if we've got societies with no members, they would show up. This would be useful for example to pinpoint societies for students interested in building a society from the ground up.

```
SELECT students.name societies.society
FROM students
RIGHT JOIN societies ON students.id = societies.student_id;
```

**FULL OUTER JOIN** - It's the combination of an INNER JOIN, LEFT OUTER JOIN, and RIGHT OUTER JOIN.

Similar to the LEFT OUTER JOIN, RIGHT OUTER JOIN, and the INNER JOIN. We would get the output of the LEFT JOIN, and the RIGHT JOIN and would be useful in being able to easily find the students that have no society, and perhaps also point out to them which societies they would be able to make the most impact in by helping create it from the ground up.

```
SELECT students.name societies.society
FROM students
FULL OUTER JOIN societies ON students.id = societies.student_id;
```

3) **Primary key** would be a key that is a unique identifier in a table, and is only found once within the entire table. This would allow you to identify a single row of data just by it's **primary key**.

A **primary key** real world example would be in a college, your student ID #. This would be your primary key in the College's `students` database table.

A **foreign key** would be a key in a table that links entries from this table to another table via the other table's *primary key*. Typically used in many to one relationships where we could link multiple of the same *primary keys* in a table by assigning it to a *foreign key* value.

A **foreign key** real world example would be your major's program ID found within your Colleges *students* table. Alternatively, the foreign key could also be a society that the student belongs to. The major ID or society ID would be the *primary key* in the College's `program major` table or the `societies` table respectively. The major ID or society ID could both be **foreign keys** in the `students` table.

4) Aliasing is utilizing a different variable name to represent a table's name or a column's name. Typically done by using the statement `AS` when declaring a `FROM` or `JOIN` statement for  representing a table name, such as:

> SELECT t.column1 
> FROM table_name AS t
> WHERE t.id = 20
> INNER JOIN table2_name;

or to represent an aliased column:

> SELECT AVG(column1) AS average_score, name
> FROM table_name
> WHERE alias1 = 20
> GROUP BY name
> ORDER BY average_score DESC;

This would allow for more readable queries, and perhaps economize keystrokes for typing if the query is particularly long and the aliased column or table is used multiple times in the query.

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

this query would yield the real names of users with emails that match in both `fantasy_gamer` table and the `charachters` table.

This is because there exists an `email_address` column in both the `fantasy_gamer` table, as well as the `charachters` table.

7) 

I created these tables:
```
contract_business=# SELECT *
contract_business-# FROM shifts
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


  name   | day_of_week | shift 
---------+-------------+-------
 Xavier  | Monday      |     1
 Xavier  | Tuesday     |     1
 Xavier  | Wednesday   |     1
 Xavier  | Thursday    |     1
 Xavier  | Friday      |     1
 Maria   | Monday      |     2
 Maria   | Tuesday     |     2
 Maria   | Wednesday   |     2
 Maria   | Thursday    |     2
 Maria   | Friday      |     2
 Roger   | Monday      |     0
 Roger   | Tuesday     |     0
 Roger   | Wednesday   |     0
 Roger   | Thursday    |     0
 Roger   | Friday      |     0
 Justine | Saturday    |     0
 Justine | Sunday      |     0
 Omar    | Saturday    |     1
 Omar    | Sunday      |     1
 Tammy   | Saturday    |     2
 Tammy   | Sunday      |     2
(21 rows)
```

OR because I used the same column name `employee_id` in both tables `employees` and `shifts`, I could use `NATURAL JOIN`, and the same output would be shown.

```
contract_business=# SELECT a.name, b.day_of_week, b.shift
FROM employees AS a
NATURAL JOIN shifts AS b;


  name   | day_of_week | shift 
---------+-------------+-------
 Xavier  | Monday      |     1
 Xavier  | Tuesday     |     1
 Xavier  | Wednesday   |     1
 Xavier  | Thursday    |     1
 Xavier  | Friday      |     1
 Maria   | Monday      |     2
 Maria   | Tuesday     |     2
 Maria   | Wednesday   |     2
 Maria   | Thursday    |     2
 Maria   | Friday      |     2
 Roger   | Monday      |     0
 Roger   | Tuesday     |     0
 Roger   | Wednesday   |     0
 Roger   | Thursday    |     0
 Roger   | Friday      |     0
 Justine | Saturday    |     0
 Justine | Sunday      |     0
 Omar    | Saturday    |     1
 Omar    | Sunday      |     1
 Tammy   | Saturday    |     2
 Tammy   | Sunday      |     2
(21 rows)
```
For the second part, I was albe to create a list of possible schedules using `CROSS JOIN`:

```
contract_business=# SELECT a.name, b.day_of_week, b.shift
FROM employees AS a
CROSS JOIN shifts AS b;

  name   | day_of_week | shift 
---------+-------------+-------
 Xavier  | Monday      |     1
 Xavier  | Tuesday     |     1
 Xavier  | Wednesday   |     1
 Xavier  | Thursday    |     1
 Xavier  | Friday      |     1
 Xavier  | Monday      |     2
 Xavier  | Tuesday     |     2
 Xavier  | Wednesday   |     2
 Xavier  | Thursday    |     2
 Xavier  | Friday      |     2
 Xavier  | Monday      |     0
 Xavier  | Tuesday     |     0
 Xavier  | Wednesday   |     0
 Xavier  | Thursday    |     0
 Xavier  | Friday      |     0
 Xavier  | Saturday    |     0
 Xavier  | Sunday      |     0
 Xavier  | Saturday    |     1
 Xavier  | Sunday      |     1
 Xavier  | Saturday    |     2
 Xavier  | Sunday      |     2
 Maria   | Monday      |     1
 Maria   | Tuesday     |     1
 Maria   | Wednesday   |     1
 Maria   | Thursday    |     1
 Maria   | Friday      |     1
 Maria   | Monday      |     2
 Maria   | Tuesday     |     2
 Maria   | Wednesday   |     2
 Maria   | Thursday    |     2
 Maria   | Friday      |     2
 Maria   | Monday      |     0
 Maria   | Tuesday     |     0
 Maria   | Wednesday   |     0
 Maria   | Thursday    |     0
 Maria   | Friday      |     0
 Maria   | Saturday    |     0
 Maria   | Sunday      |     0
 Maria   | Saturday    |     1
 Maria   | Sunday      |     1
 Maria   | Saturday    |     2
 Maria   | Sunday      |     2
 Roger   | Monday      |     1
 Roger   | Tuesday     |     1
 Roger   | Wednesday   |     1
 Roger   | Thursday    |     1
 Roger   | Friday      |     1
 Roger   | Monday      |     2
 Roger   | Tuesday     |     2
 Roger   | Wednesday   |     2
 Roger   | Thursday    |     2
 Roger   | Friday      |     2
 Roger   | Monday      |     0
 Roger   | Tuesday     |     0
 Roger   | Wednesday   |     0
 Roger   | Thursday    |     0
 Roger   | Friday      |     0
 Roger   | Saturday    |     0
 Roger   | Sunday      |     0
 Roger   | Saturday    |     1
 Roger   | Sunday      |     1
 Roger   | Saturday    |     2
 Roger   | Sunday      |     2
 Justine | Monday      |     1
 Justine | Tuesday     |     1
 Justine | Wednesday   |     1
 Justine | Thursday    |     1
 Justine | Friday      |     1
 Justine | Monday      |     2
 Justine | Tuesday     |     2
 Justine | Wednesday   |     2
 Justine | Thursday    |     2
 Justine | Friday      |     2
 Justine | Monday      |     0
 Justine | Tuesday     |     0
 Justine | Wednesday   |     0
 Justine | Thursday    |     0
 Justine | Friday      |     0
 Justine | Saturday    |     0
 Justine | Sunday      |     0
 Justine | Saturday    |     1
 Justine | Sunday      |     1
 Justine | Saturday    |     2
 Justine | Sunday      |     2
 Omar    | Monday      |     1
 Omar    | Tuesday     |     1
 Omar    | Wednesday   |     1
 Omar    | Thursday    |     1
 Omar    | Friday      |     1
 Omar    | Monday      |     2
 Omar    | Tuesday     |     2
 Omar    | Wednesday   |     2
 Omar    | Thursday    |     2
 Omar    | Friday      |     2
 Omar    | Monday      |     0
 Omar    | Tuesday     |     0
 Omar    | Wednesday   |     0
 Omar    | Thursday    |     0
 Omar    | Friday      |     0
 Omar    | Saturday    |     0
 Omar    | Sunday      |     0
 Omar    | Saturday    |     1
 Omar    | Sunday      |     1
 Omar    | Saturday    |     2
 Omar    | Sunday      |     2
 Tammy   | Monday      |     1
 Tammy   | Tuesday     |     1
 Tammy   | Wednesday   |     1
 Tammy   | Thursday    |     1
 Tammy   | Friday      |     1
 Tammy   | Monday      |     2
 Tammy   | Tuesday     |     2
 Tammy   | Wednesday   |     2
 Tammy   | Thursday    |     2
 Tammy   | Friday      |     2
 Tammy   | Monday      |     0
 Tammy   | Tuesday     |     0
 Tammy   | Wednesday   |     0
 Tammy   | Thursday    |     0
 Tammy   | Friday      |     0
 Tammy   | Saturday    |     0
 Tammy   | Sunday      |     0
 Tammy   | Saturday    |     1
 Tammy   | Sunday      |     1
 Tammy   | Saturday    |     2
 Tammy   | Sunday      |     2
(126 rows)
```

8) 

SELECT v.name, d.name
FROM volunteers AS v
LEFT OUTER JOIN dogs AS d
ON v.foster_id = d.id;

SELECT a.first_name, a.last_name, d.name, c.name
FROM adoptions AS ad
LEFT JOIN dogs AS d ON d.id = ad.dog
LEFT JOIN cats AS c ON c.id = ad.cat
JOIN adopters AS a ON ad.adopter = a.id

SELECT a.first_name, a.last_name, d.name
FROM adopters AS a
LEFT JOIN adoptions AS ad ON a.id = ad.adopter
CROSS JOIN dogs AS d
WHERE ad.id IS NULL AND d.adoption_date IS NULL;


SELECT d.name AS dog, c.name AS cat  
FROM adoptions as ad
FULL OUTER JOIN cats AS c ON ad.cat = c.id
FULL OUTER JOIN dogs AS d ON ad.dog = d.id
WHERE ad.dog IS NULL AND ad.cat IS NULL;

SELECT v.name, d.name
FROM volunteers AS v
FULL OUTER JOIN dogs AS d ON v.foster_id = d.id
WHERE adoption_date IS NULL;

9)

SELECT a.first_name, a.last_name
FROM adoptions
INNER JOIN cats AS c ON c.id = adoptions.cat
INNER JOIN adopters AS a ON a.id = adoptions.adopter
WHERE c.name = 'Seashell';

10)

SELECT p.name h.rank
FROM holds AS h
JOIN patrons AS p ON p.id = h.user_id
JOIN books AS b ON b.isbn = h.isbn
WHERE b.title = 'Harry Potter and the Sourcerer''s Stone'
ORDER BY rank ASC;

SELECT b.title, t.checked_out_date
FROM books AS b
LEFT JOIN transactions AS t ON t.isbn = b.isbn
WHERE checked_in_date IS NULL;

SELECT AVG(t.checked_in_date::timestamp - t.checked_out_date::timestamp) AS read_time, b.title 
FROM books AS b
JOIN transactions AS t ON t.isbn = b.isbn
WHERE t.checked_out_date >= '2017-08-12' AND t.checked_in_date IS NOT NULL
GROUP BY b.title;

SELECT b.title, t.checked_out_date
FROM books AS b
LEFT JOIN transactions AS t ON b.isbn = t.isbn
WHERE t.checked_out_date >= '2012-09-15';

SELECT p.name, b.title
FROM patrons AS p
LEFT JOIN transactions AS t ON (p.id = t.user_id AND t.checked_in_date IS NULL)
LEFT JOIN books AS b ON b.isbn = t.isbn
WHERE b.title IS NOT NULL;

