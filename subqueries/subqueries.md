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
