1)

A subquery is, essentially, a query within a query in SQL. A regular SQL query becomes a query when it is inserted into the `SELECT`, `FROM`, or `WHERE` clauses of a query. The subquery allows it's returned data to be used by the query it's found inside of.

2)

A subquery within a `SELECT` statement can be used when the results of a query should not be changed or influenced by the results of the subquery.

3)

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
b)
```
SELECT employees.name, shifts.day_of_week, shifts.shift
FROM employees, shifts;
```

8)

a)

```
SELECT volunteers.name, 
  (SELECT dogs.name
  FROM dogs
  WHERE dogs.id = volunteers.foster_id)
FROM volunteers;
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

c)

```
SELECT adopters.first_name, adopters.last_name, dogs.name
FROM adopters, dogs
WHERE adopters.id NOT IN 
  (SELECT adopter FROM adoptions);
```

d) ??????

```
SELECT dogs.name AS dog_name, NULL AS cat_name
FROM dogs
WHERE NOT EXISTS 
  (SELECT adoptions.dog 
  FROM adoptions 
  WHERE dogs.id = adoptions.dog)
UNION
SELECT NULL AS dog_name, cats.name AS cat_name
FROM cats
WHERE NOT EXISTS 
  (SELECT adoptions.cat 
  FROM adoptions 
  WHERE cats.id = adoptions.cat);
```

e)

```
SELECT name, 
  (SELECT dogs.name 
  FROM dogs 
  WHERE volunteers.foster_id = dogs.id)
FROM volunteers;
```

9) 

```
SELECT adopters.first_name, adopters.last_name
FROM 
  (SELECT cats.id, cats.name, adoptions.adopter 
  FROM adoptions, cats 
  WHERE adoptions.cat = cats.id) 
  AS adopted_cats, adopters
WHERE adopted_cats.name = 'Seashell' AND adopted_cats.adopter = adopters.id;
```

10)

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

c) 

```
SELECT COUNT(transaction_dates.checked_out_date) AS times_checked_out, books.title
FROM books, 
  (SELECT checked_out_date, isbn FROM transactions) 
  AS transaction_dates
WHERE transaction_dates.isbn = books.isbn AND transaction_dates.checked_out_date >= '2017-9-9'
GROUP BY books.title
ORDER BY number_of_times_checked_out DESC;
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

11)

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


c)

I didn't really need to use a subquery for this one.

```
SELECT total_revenue                                                                                           
FROM transactions, flights
WHERE transactions.flight_number = flights.flight_number AND (flights.destination = 'ATL' OR flights.origin = 'ATL');
```


12)

The `JOIN` statements are easier to read in my opinion. There's a lot less clutter in reading a join and because the `JOIN` encompasses the idea of two tables being joined together, it makes it easier to read.

The subqueries are easier to write in my opinion. Mainly because I can write a query for a part of what I'm looking for, and then start chunking down into more granular detail towards what I want with another query.
