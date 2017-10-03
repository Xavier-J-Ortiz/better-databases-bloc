1)

A subquery is, essentially, a query within a query in SQL. A regular SQL query becomes a query when it is inserted into the `SELECT`, `FROM`, or `WHERE` clauses of a query. The subquery allows it's returned data to be used by the query it's found inside of.

2)

A subquery within a `SELECT` statement can be used when the results of a query should not be changed or influenced by the results of the subquery.

3)

An **inline views** is a subquery within the `FROM` clause. I would use **inline views** when I'd like to query the results table with another query.

A **nested query** would be a query in a `WHERE` clause. I would use the **nested query** in the event that I'd like to fine tune filter a query with a `WHERE` clause. By using a subquery and fetching data from another table, the subquery results would be used as the filter variables. The subquery table could have meaningful information available that was unavailable in the main query's table. We could then leverage the subquery table's information to filter the main query's table.

Finally, subqueries in a `SELECT` clause are typically used when you want to get a function from a subquery but do not want to aggregate the function to apply to the main query. An example of this is if we want to get the average age of a table representing a group of people in the subquery, and aggegate that data in the main query of the same table except the query listing the name and age of the person. The output would show the average age of the group, the name, and age of each person in every row.
