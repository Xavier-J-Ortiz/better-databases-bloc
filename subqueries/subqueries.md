1)

A subquery is, essentially, a query within a query in SQL. A regular SQL query becomes a query when it is inserted into the `SELECT`, `FROM`, or `WHERE` clauses of a query. The subquery allows it's returned data to be used by the query it's found inside of.

2)

A subquery within a `SELECT` statement can be used when the results of a query should not be changed or influenced by the results of the subquery.

3)

I would use a subquery if I needed to use the data from at least a section or part of another table (lets call it table_b) in conjunction with a main table (lets call this table_a) and vice versa. If the elements from table_b are crucial to filtering, complementing, or enhancing the data from table_a, I would use a subquery.

An **inline views** is a subquery within the `FROM` clause. I would use **inline views** when I'd like to query the results table with another query.

A **nested query** would be a query in a `WHERE` clause. I would use the **nested query** in the event that I'd like to fine tune filter a query with a `WHERE` clause. By using a subquery and fetching data from another table, the subquery results would be used as the filter variables. The subquery table could have meaningful information available that was unavailable in the main query's table. We could then leverage the subquery table's information to filter the main query's table.

Finally, subqueries in a `SELECT` clause are typically used when you want to get a function from a subquery but do not want to aggregate the function to apply to the main query. An example of this is if we want to get the average age of a table representing a group of people in the subquery, and aggegate that data in the main query of the same table except the query listing the name and age of the person. The output would show the average age of the group, the name, and age of each person in every row.

4)

Also known as a vector expression. A row constructor is an expression that builds a row by explicitly specifying the values to be assigned for it's column fields. In other words, it isan expression that builds a row value.

5)

if there is a `NULL` value in the comparison, the comparison will return `NULL`

6)

There are a couple of ways that subqueries can be used within a where clause. They would use the following words shown below, in conjuction with the `WHERE` clause and , and/or with a specified row to match.

EXISTS - when used `WHERE EXISTS <subquery>`, if the subquery returns at least one row, the `EXISTS` returns `true`. Else, it returns `false`.

NOT EXISTS - when used `WHERE NOT EXISTS <subquery>`, if the subquery does not return at least one row, the `NOT EXISTS` returns `true`. Else, it returns `false`

IN - when used in a `WHERE` clause like so `WHERE <column name> IN <subquery>`, the result of the `IN` is true if any matching subquery row is found. Else, the result is false. If no rows are returned, the result is also false. If the value to the left of the `IN`, in this case `<column name>`,  evaluates to `NULL`, the result will also be `NULL`.

NOT IN - when used in a `WHERE` clause like so `WHERE <column name> NOT IN <subquery>`, the result of the `NOT IN` is true if _no_ matching subquery rows are found. If there's one or more matches resulting from the subquery, the result is false. If no rows are returned, the result is true. If the value to the left of the `NOT IN`, in this case `<column name>`,  evaluates to `NULL`, the result will also be `NULL`.

ANY - when used in a where clause `WHERE <column name> <comparison element> ANY <subquery>`, returns a `true` or `false` if one or more subquery rows are found to match a given expression. 

SOME - `SOME` is the same as `ANY`, and has the same behavior. They can be used interchageably however you prefer.

ALL - when used in a where clause like so `WHERE <column name> <comparison element> ALL <subquery>`, returns `true` if _every_ result returned by the subquery matches the given condition, _or_ the subquery returns no rows.

7)
a)
```
SELECT * FROM employees;

 employee_id |  name   | gender 
-------------+---------+--------
           0 | Xavier  | M
           1 | Maria   | F
           2 | Roger   | M
           3 | Justine | F
           4 | Omar    | M
           5 | Tammy   | F
```

```
SELECT *
ORDER BY shift_id ASC;
 shift_id | day_of_week | shift 
----------+-------------+-------
        0 | Sunday      |     0
        1 | Sunday      |     1
        2 | Sunday      |     2
        3 | Monday      |     0
        4 | Monday      |     1
        5 | Monday      |     2
        6 | Tuesday     |     0
        7 | Tuesday     |     1
        8 | Tuesday     |     2
        9 | Wednesday   |     0
       10 | Wednesday   |     1
       11 | Wednesday   |     2
       12 | Thursday    |     0
       13 | Thursday    |     1
       14 | Thursday    |     2
       15 | Friday      |     0
       16 | Friday      |     1
       17 | Friday      |     2
       18 | Saturday    |     0
       19 | Saturday    |     1
       20 | Saturday    |     2
```


```
SELECT s.shift_id, e.employee_id
FROM shifts AS s
FULL JOIN (SELECT employee_id FROM employees) AS e ON FALSE
contract_business-# ORDER BY shift_id;
```

```
 shift_id | employee_id 
----------+-------------
        0 |            
        1 |            
        2 |            
        3 |            
        4 |            
        5 |            
        6 |            
        7 |            
        8 |            
        9 |            
       10 |            
       11 |            
       12 |            
       13 |            
       14 |            
       15 |            
       16 |            
       17 |            
       18 |            
       19 |            
       20 |            
          |           2
          |           5
          |           1
          |           4
          |           0
          |           3
(27 rows)
```

b)
```
SELECT employees.name, reduced_shifts.day_of_week, reduced_shifts.shift
FROM employees, (SELECT day_of_week, shift FROM shifts) AS reduced_shifts;
```

```
  name   | day_of_week | shift 
---------+-------------+-------
 Xavier  | Monday      |     1
 Maria   | Monday      |     1
 Roger   | Monday      |     1
 Justine | Monday      |     1
 Omar    | Monday      |     1
 Tammy   | Monday      |     1
 Xavier  | Tuesday     |     1
 Maria   | Tuesday     |     1
 Roger   | Tuesday     |     1
 Justine | Tuesday     |     1
 Omar    | Tuesday     |     1
 Tammy   | Tuesday     |     1
 Xavier  | Wednesday   |     1
 Maria   | Wednesday   |     1
 Roger   | Wednesday   |     1
 Justine | Wednesday   |     1
 Omar    | Wednesday   |     1
 Tammy   | Wednesday   |     1
 Xavier  | Thursday    |     1
 Maria   | Thursday    |     1
 Roger   | Thursday    |     1
 Justine | Thursday    |     1
 Omar    | Thursday    |     1
 Tammy   | Thursday    |     1
 Xavier  | Friday      |     1
 Maria   | Friday      |     1
 Roger   | Friday      |     1
 Justine | Friday      |     1
 Omar    | Friday      |     1
 Tammy   | Friday      |     1
 Xavier  | Monday      |     2
 Maria   | Monday      |     2
 Roger   | Monday      |     2
 Justine | Monday      |     2
 Omar    | Monday      |     2
 Tammy   | Monday      |     2
 Xavier  | Tuesday     |     2
 Maria   | Tuesday     |     2
 Roger   | Tuesday     |     2
 Justine | Tuesday     |     2
 Omar    | Tuesday     |     2
 Tammy   | Tuesday     |     2
 Xavier  | Wednesday   |     2
 Maria   | Wednesday   |     2
 Roger   | Wednesday   |     2
 Justine | Wednesday   |     2
 Omar    | Wednesday   |     2
 Tammy   | Wednesday   |     2
 Xavier  | Thursday    |     2
 Maria   | Thursday    |     2
 Roger   | Thursday    |     2
 Justine | Thursday    |     2
 Omar    | Thursday    |     2
 Tammy   | Thursday    |     2
 Xavier  | Friday      |     2
 Xavier  | Friday      |     2
 Maria   | Friday      |     2
 Roger   | Friday      |     2
 Justine | Friday      |     2
 Omar    | Friday      |     2
 Tammy   | Friday      |     2
 Xavier  | Monday      |     0
 Maria   | Monday      |     0
 Roger   | Monday      |     0
 Justine | Monday      |     0
 Omar    | Monday      |     0
 Tammy   | Monday      |     0
 Xavier  | Tuesday     |     0
 Maria   | Tuesday     |     0
 Roger   | Tuesday     |     0
 Justine | Tuesday     |     0
 Omar    | Tuesday     |     0
 Tammy   | Tuesday     |     0
 Xavier  | Wednesday   |     0
 Maria   | Wednesday   |     0
 Roger   | Wednesday   |     0
 Justine | Wednesday   |     0
 Omar    | Wednesday   |     0
 Tammy   | Wednesday   |     0
 Xavier  | Thursday    |     0
 Maria   | Thursday    |     0
 Roger   | Thursday    |     0
 Justine | Thursday    |     0
 Omar    | Thursday    |     0
 Tammy   | Thursday    |     0
 Xavier  | Friday      |     0
 Maria   | Friday      |     0
 Roger   | Friday      |     0
 Justine | Friday      |     0
 Omar    | Friday      |     0
 Tammy   | Friday      |     0
 Xavier  | Saturday    |     0
 Maria   | Saturday    |     0
 Roger   | Saturday    |     0
 Justine | Saturday    |     0
 Omar    | Saturday    |     0
 Tammy   | Saturday    |     0
 Xavier  | Sunday      |     0
 Maria   | Sunday      |     0
 Roger   | Sunday      |     0
 Justine | Sunday      |     0
 Omar    | Sunday      |     0
 Tammy   | Sunday      |     0
 Xavier  | Saturday    |     1
 Maria   | Saturday    |     1
 Roger   | Saturday    |     1
 Justine | Saturday    |     1
 Omar    | Saturday    |     1
 Tammy   | Saturday    |     1
 Xavier  | Sunday      |     1
 Maria   | Sunday      |     1
 Roger   | Sunday      |     1
 Justine | Sunday      |     1
 Omar    | Sunday      |     1
 Tammy   | Sunday      |     1
 Xavier  | Saturday    |     2
 Maria   | Saturday    |     2
 Roger   | Saturday    |     2
 Justine | Saturday    |     2
 Omar    | Saturday    |     2
 Tammy   | Saturday    |     2
 Xavier  | Sunday      |     2
 Maria   | Sunday      |     2
 Roger   | Sunday      |     2
 Justine | Sunday      |     2
 Omar    | Sunday      |     2
 Tammy   | Sunday      |     2

```

8)

Tables in this question:

```
cats=# SELECT * FROM cats; SELECT * FROM dogs; SELECT * FROM adoptions; SELECT * FROM adopters;
 id |   name   | gender | age | intake_date | adoption_date 
----+----------+--------+-----+-------------+---------------
  1 | Mushi    | M      |   1 | 2016-01-09  | 2016-03-22
  2 | Seashell | F      |   7 | 2016-01-09  | 
  3 | Azul     | M      |   3 | 2016-01-11  | 2016-04-17
  4 | Victoire | M      |   7 | 2016-01-11  | 2016-09-01
  5 | Nala     | F      |   1 | 2016-01-12  | 
  6 | Falafel  | F      |   3 | 2015-06-13  | 
(6 rows)

 id |   name    | gender | age | weight | intake_date |         breed         | in_foster | adoption_date 
----+-----------+--------+-----+--------+-------------+-----------------------+-----------+---------------
  1 | Rufus     | M      |   3 |     30 | 2017-01-01  | Shepard Lab Mix       | N         | 
  2 | Rex       | M      |   1 |     10 | 2016-10-11  | Boxer                 | Y         | 
  3 | Raspberry | F      |   2 |     20 | 2017-03-21  | Poodle                | N         | 
  6 | Stinker   | M      |   4 |     37 | 2017-04-20  | Pitbull Rotweiler Mix | N         | 
  4 | Pookie    | F      |  13 |      8 | 2017-03-20  | Chihuahua             | A         | 2017-03-30
  5 | Candy     | F      |   1 |      9 | 2017-04-20  | Doberman Pinscher     | A         | 2017-04-30
(6 rows)

 id | cat | dog |  fee   |    date    | adopter 
----+-----+-----+--------+------------+---------
  1 |   1 |     | $50.00 | 2016-03-22 |       1
  2 |   3 |     | $50.00 | 2016-09-22 |       2
  3 |   4 |     | $10.00 | 2017-10-22 |       3
  4 |     |   4 | $10.00 | 2017-03-30 |       4
  5 |     |   5 | $50.00 | 2017-04-30 |       6
(5 rows)

 first_name | last_name |        address         | phone_number | id 
------------+-----------+------------------------+--------------+----
 Maria      | Ortiz     | 234 92nd Street        |      5552789 |  1
 Thomas     | Muller    | 234 Millionaire Avenue |      5552333 |  2
 Franz      | Ferdinand | 149 Rockers Road       |      5553111 |  3
 Old        | Lady      | 540 Musty Inlet        |      5559009 |  4
 Young      | Lad       | 540 Energy Street      |      5559109 |  5
 Walter     | Mathau    | 540 Grumpy Hwy         |      5559129 |  6
(6 rows)
```

a)

```
SELECT volunteers.name AS volunteer_name, 
  (SELECT dogs.name
  FROM dogs
  WHERE dogs.id = volunteers.foster_id) AS dog_name
FROM volunteers;
```

Results:

```
 volunteer_name | dog_name 
----------------+----------
 Xavier Ortiz   | 
 Rachel Budde   | Rex
 Cara Klewin    | 
(3 rows)
```

b)

```
SELECT adopters.first_name, adopters.last_name, 
  (SELECT name FROM dogs WHERE adoptions.dog = id) AS dog, 
  (SELECT name FROM cats WHERE adoptions.cat = cats.id) AS cat
FROM adopters, 
  (SELECT cat, dog, adopter FROM adoptions WHERE date >= '2017-9-8') AS adoptions
WHERE adoptions.adopter = adopters.id;
```

Results:
```
 first_name | last_name | dog |   cat    
------------+-----------+-----+----------
 Franz      | Ferdinand |     | Victoire

```
c)

```
SELECT adopters.first_name, adopters.last_name, dogs.name
FROM adopters, dogs
WHERE adopters.id NOT IN 
  (SELECT adopter FROM adoptions);
```

Results:
```
 first_name | last_name |   name    
------------+-----------+-----------
 Young      | Lad       | Rufus
 Young      | Lad       | Rex
 Young      | Lad       | Raspberry
 Young      | Lad       | Stinker
 Young      | Lad       | Pookie
 Young      | Lad       | Candy

```
d) 

```
SELECT  concat(dogs.name, c.name) AS unadopted_animals
FROM dogs
FULL JOIN (SELECT cats.name
FROM cats
WHERE id NOT IN
(SELECT adoptions.cat 
FROM adoptions
WHERE adoptions.cat IS NOT NULL)) AS c ON FALSE
WHERE dogs.adoption_date IS NULL;
```

```
 unadopted_animals 
-------------------
 Seashell
 Nala
 Falafel
 Rufus
 Rex
 Raspberry
 Stinker
(7 rows)
```

e)

```
SELECT v.name AS volunteer_name, dogs.name AS dog_name
FROM (SELECT name, foster_id FROM volunteers) AS v
FULL JOIN dogs ON dogs.id = v.foster_id;
```

```
 volunteer_name | dog_name  
----------------+-----------
                | Rufus
 Rachel Budde   | Rex
                | Raspberry
                | Stinker
                | Pookie
                | Candy
 Cara Klewin    | 
 Xavier Ortiz   | 
(8 rows)
```


9) 

Nobody adopted Seashells yet. So I chose Mushi for this example.

```
SELECT adopters.first_name, adopters.last_name
FROM 
  (SELECT cats.id, cats.name, adoptions.adopter 
  FROM adoptions, cats 
  WHERE adoptions.cat = cats.id) 
  AS adopted_cats, adopters
WHERE adopted_cats.name = 'Mushi' AND adopted_cats.adopter = adopters.id;
```

Results:
```
 first_name | last_name 
------------+-----------
 Maria      | Ortiz
```

10)

Tables:
```
books=# select * from books; select * from holds; select * from patrons; select * from transactions;
 isbn |                     title                      |      author      
------+------------------------------------------------+------------------
    1 | Harry Potter and the Sourcerer's Stone         | J.K. Rowlings
    2 | 20,000 Leagues Under the Sea                   | Jules Verne
    3 | Trials and Tribulations of a Engineer in Sales | Xavier Ortiz
    4 | It                                             | Stephen King
    5 | Jurassic Park                                  | Micheal Crichton
(5 rows)

 id | isbn | user_id | rank |    date    
----+------+---------+------+------------
  1 |    1 |       1 |    1 | 2017-03-19
  2 |    1 |       2 |    3 | 2017-02-09
  3 |    1 |       3 |    2 | 2016-11-09
  4 |    3 |       4 |    1 | 2016-11-09
  5 |    5 |       5 |    1 | 2016-10-23
(5 rows)

 id |        name         | fine_amount 
----+---------------------+-------------
  1 | Xavier Ortiz        |       $0.00
  2 | Maria Andrea        |       $3.00
  3 | Maria Eugenia Otero |       $1.00
  4 | Dave Anthony        |       $1.00
  5 | Isabel Short        |       $4.00
(5 rows)

 id | checked_out_date | checked_in_date | user_id | isbn 
----+------------------+-----------------+---------+------
  4 | 2015-04-23       | 2015-05-22      |       1 |    2
  5 | 2016-02-23       | 2016-07-02      |       2 |    4
  6 | 2014-02-23       | 2014-07-02      |       1 |    5
  7 | 2014-09-23       | 2014-11-02      |       5 |    1
  1 | 2015-02-02       | 2015-02-04      |       4 |    1
  3 | 2015-04-13       | 2015-04-16      |       5 |    5
 11 | 2017-09-10       | 2017-09-12      |       4 |    2
 12 | 2017-09-20       | 2017-09-22      |       5 |    2
 13 | 2017-09-10       | 2017-09-22      |       1 |    5
 14 | 2017-09-10       | 2017-09-22      |       2 |    4
  2 | 2015-03-03       | 2015-03-05      |       3 |    1
  8 | 2017-09-10       | 2017-09-12      |       1 |    1
  9 | 2017-09-15       | 2017-09-20      |       2 |    1
 10 | 2017-09-24       | 2017-09-28      |       4 |    1
 15 | 2017-10-16       |                 |       1 |    4
 16 | 2017-10-16       |                 |       5 |    5
(16 rows)
```

a)

```
SELECT patrons.name
FROM 
  patrons,
    (SELECT books.isbn
    FROM books
    WHERE title = 'Harry Potter and the Sourcerer''s Stone') 
  AS stone_isbn,
  holds
WHERE 
  patrons.id = holds.user_id 
  AND 
  holds.isbn = stone_isbn.isbn
ORDER BY holds.rank
```

Results:
```
        name         
---------------------
 Xavier Ortiz
 Maria Eugenia Otero
 Maria Andrea
```

b)

```
SELECT books.title, 
  (SELECT checked_in_date IS NULL 
  FROM transactions 
  WHERE checked_in_date IS NULL 
  AND books.isbn = transactions.isbn) 
  AS is_checked_out
FROM books;
```

Results:

```
                     title                      | is_checked_out 
------------------------------------------------+----------------
 Harry Potter and the Sourcerer's Stone         | 
 20,000 Leagues Under the Sea                   | 
 Trials and Tribulations of a Engineer in Sales | 
 It                                             | t
 Jurassic Park                                  | t
```

c) 

```
SELECT COUNT(transaction_dates.checked_out_date) AS times_checked_out, books.title
FROM books, 
  (SELECT checked_out_date, isbn FROM transactions) 
  AS transaction_dates
WHERE transaction_dates.isbn = books.isbn AND transaction_dates.checked_out_date >= '2017-9-9'
GROUP BY books.title
ORDER BY times_checked_out DESC;
```

```
 times_checked_out |                 title                  
-------------------+----------------------------------------
                 3 | Harry Potter and the Sourcerer's Stone
                 2 | 20,000 Leagues Under the Sea
                 2 | It
                 2 | Jurassic Park
```
d)

```
SELECT books.title
FROM books
WHERE books.title NOT IN 
  (SELECT books.title
  FROM books, 
    (SELECT checked_out_date, isbn 
    FROM transactions) AS transaction_dates
  WHERE 
    books.isbn = transaction_dates.isbn 
    AND 
    transaction_dates.checked_out_date >= '2012-10-9');
```

```
                     title                      
------------------------------------------------
 Trials and Tribulations of a Engineer in Sales
```
e)

```
SELECT patrons.name, 
  (SELECT books.title
  FROM books, transactions
  WHERE books.isbn = transactions.isbn 
    AND 
  patrons.id = transactions.user_id 
    AND 
  checked_in_date IS NULL)
FROM patrons;
```

```
        name         |     title     
---------------------+---------------
 Xavier Ortiz        | It
 Maria Andrea        | 
 Maria Eugenia Otero | 
 Dave Anthony        | 
 Isabel Short        | Jurassic Park
```

11)
Tables:

```
airliner=# SELECT * FROM airplanes; SELECT * FROM flights; SELECT * FROM transactions;
 model | seat_capacity | range 
-------+---------------+-------
   747 |           500 |  5000
   727 |           400 |  5000
   707 |           300 |  5000
   181 |           400 | 10000
   121 |            12 | 10000
(5 rows)

 flight_number | destination | origin |      company      | distance | flight_time |    date    | airplane_model 
---------------+-------------+--------+-------------------+----------+-------------+------------+----------------
             1 | DOR         | JFK    | Delta             |     3500 | 07:00:00    | 2017-12-01 |            747
             2 | JFK         | HRW    | Virgin Air        |     5000 | 09:00:00    | 2017-11-23 |            727
             3 | EWR         | ATL    | American Airlines |     1000 | 10:00:00    | 2017-10-23 |            727
             6 | JFK         | PDX    | American Airlines |     5000 | 10:00:00    | 2017-11-03 |            121
             9 | PDX         | JFK    | American Airlines |     4000 | 01:00:00    | 2017-11-22 |            181
             4 | ATL         | EWR    | Delta             |     1000 | 13:00:00    | 2017-12-11 |            727
             7 | SFO         | PDX    | Delta             |     5000 | 10:00:00    | 2017-12-11 |            121
            10 | SFO         | JFK    | Delta             |     2000 | 01:00:00    | 2017-12-11 |            181
             5 | SFO         | JFK    | Virgin Air        |     5000 | 10:00:00    | 2017-10-14 |            707
             8 | PDX         | SFO    | Virgin Air        |     1000 | 16:00:00    | 2017-10-14 |            121
            11 | DOR         | JFK    | Delta             |     3500 | 07:00:00    | 2017-11-12 |            747
(11 rows)

 id | seats_sold | total_revenue | total_cost | flight_number |    date    
----+------------+---------------+------------+---------------+------------
  2 |        300 |    $30,000.00 |  $5,000.00 |             2 | 2017-11-23
  3 |        200 |    $20,000.00 |  $3,000.00 |             3 | 2017-10-23
  5 |         80 |     $8,000.00 |  $1,100.00 |             5 | 2017-10-14
  6 |         12 |     $7,500.00 |  $1,000.00 |             6 | 2017-11-03
  7 |         12 |    $12,500.00 |  $1,400.00 |             7 | 2017-12-11
  8 |         12 |    $12,500.00 |    $800.00 |             8 | 2017-10-14
  9 |        200 |    $12,500.00 |    $800.00 |             9 | 2017-11-22
 10 |        200 |    $12,000.00 |    $300.00 |            10 | 2017-12-11
  4 |        120 |     $2,000.00 |    $900.00 |             4 | 2017-12-11
  1 |        101 |     $3,000.00 |  $1,000.00 |             1 | 2017-12-01
 11 |         99 |     $7,000.00 |    $900.00 |            11 | 2017-11-12
(11 rows)
```

a)

```
SELECT DISTINCT airplane_model
FROM flights AS f1
WHERE 100 <= ALL (SELECT seats_sold
FROM transactions, flights
WHERE transactions.flight_number = flights.flight_number
AND f1.airplane_model = flights.airplane_model
AND transactions.date >= '9/10/2017');
```

Results:

```
 airplane_model 
----------------
            181
            727
```

b)

```
SELECT plane_seat_data.destination, plane_seat_data.origin
FROM transactions, 
  (SELECT flights.airplane_model, airplanes.seat_capacity, flights.flight_number, flights.destination, flights.origin
  FROM airplanes, flights
  WHERE flights.airplane_model = airplanes.model) 
  AS plane_seat_data
WHERE transactions.flight_number = plane_seat_data.flight_number AND
(transactions.seats_sold/CAST(plane_seat_data.seat_capacity AS float)) >= 0.90
AND transactions.date >= '9/10/2017';
```
Results:

```
 destination | origin 
-------------+--------
 JFK         | PDX
 SFO         | PDX
 PDX         | SFO
```

c)


```
SELECT total_revenue
FROM (SELECT total_revenue, flight_number FROM transactions) AS t, (SELECT flight_number, destination, origin FROM flights) AS f
WHERE t.flight_number = f.flight_number AND (f.destination = 'ATL' OR f.origin = 'ATL');

```

Results:
```
 total_revenue 
---------------
    $20,000.00
     $2,000.00
```


12)

The `JOIN` statements are easier to read in my opinion. There's a lot less clutter in reading a join and because the `JOIN` encompasses the idea of two tables being joined together, it makes it easier to read.

The subqueries are easier to write in my opinion, though multiple subqueries in a query can lead to it reading in a messy way. The ease of writing a subquery comes from being able to write it piece by piece with what you're looking for, then start chunking down into more granular detail towards what I want with another query.
