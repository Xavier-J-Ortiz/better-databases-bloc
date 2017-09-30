1) 

Normalization helps with avoiding duplicate and redundant information, and helps organize information in such a way that there are tangible benefits in data integrity by avoiding mistakes, helps with creating cleaner, more efficient joins and sorting, and improved performance of the database.

A real world example of where normalization is necessary, is if you have a department store business, and you have a single data set with employee data, the section they are assigned to (men's shoes, women's perfumes), a well as the store location they work in. Since everything is in a single repository, searching for all store locations would be cumbersome as it would have to look at every single employee record to make sure that it isn't a unique store location, and if it is unique, take note of the location. Also, the employee data might not take into account a store location that is brand new and about to open in a month, but doesn't have any employees hired yet.

A separate department store repository would be helpful in discerning all the available department stores and their locations.

2)

i) The first normal form says that you must not have multiple values in a column of a table.

So in our employees table from the previous example, if a store has an employee working in multiple departments, and each department they worked for was found within a single column, it would be better to flatten out this data so that _each_ department gets it's own row within a column, instead of concatenating all departments for an employee under a column.

ii) The second normal form requires the previous normal form, that is, that there is a single value within a column of a table, to start. It also requires that all _non-key_ attributes not depend on a subset of the primary key.

What this means is that a non key attribute of a table should be fully dependant on the primary key of the table.

A real world example of this is in a puchases table, where there is a column for the CustomerID, StoreID, and purchase location. The candidate keys here would be CustomerID and StoreID, but the purchase location would depend on the StoreID only, not the CustomerID. Said in other words, the location would depend on part of the primary key (StoreID), but ignore the other part of the primary key (CustomerID). To remedy this, we should create a table that has just the CustomerID, and StoreID columns, and another table that has the store location and the StoreID as the primary key.

iii) The third normal form requires that the table be in second normal form, and also requires that all transitive functional dependencies of non prime attributes must not exist.

What this means is, there should only be information that is functionally dependant on the primary key alone, and there shouldn't be a relationship between two columns proxied between a column. 

An example of a third normal form use in the real world is as follows. If there's a table with a city_id, a city_dweller, and dweller's pet, there's a relationship between the city_id and the dweller's pet, and the relationship is proxied by the city_dweller.

To fix this problem we split this table into 2 tables. One where with the cityID and the city dweller, and the other with the city dweller and the pet.

iv) Boyce Codd normal form. It's similar to third normal form, except that for every dependency, A-->B, A must be the superkey of the table. A super key is defined as a key that defines all other attributes in that table. So in a sense it's a stricter third form.
real
A real world example of this is if say there's a table with an author column, a birthplace column, a book title column, a pages column, and a first publication column. Here we have two clear top level or apex dependencies. Author --> birthplace, and book title --> first publication, pages. Because of this, and to follow BCNF, we would need that Author, and Book Title be the superkeys of their tables. So one table would have an author column and a birthplace column, the second table would have a book title column, a pages column, and a first published column. Finally, in order to link the two tables together, we would need a third column soley compromised of the superkeys.

3)

The table is already in 1NF. Lets start to turn it into 2NF.

For 2NF, the table must be in 1NF, which it already is, and all non-key attributes cannot be dependent on a subset of the primary key.

In the table, I see 3 key candidates. `entry_id`, `student_id`, and `professor_id`. The relationships between the keys are as follows **professor_id** --> {professor_name, subject}, **student_id** --> {student_email, student_name}. That leaves the the grade value. The grade value depends on the {**student_id**, **professor_id**} --> grade. This should not be as a non-key attribute cannot be dependent on a subset of the primary key.

What we should do is divide the entries up in the following tables. The first table could be the `students` table, which has the columns `student_id`, `student_email` and `student_name`, the second table could be the `professors` table, which has the columns professor_id, professor_name, and subject. In both of these tables the non key attributes wholly depend on a single primary key, keeping with the 2NF requirement.

However, we would also need a grades table. The grades table would have an entry_id, a student_id, a professor_id, as well as score column. The relationship between the keys would be {entry_id, student_id, professor_id} ---> score. This would make these tables 2NF.

In order to turn this into 3NF, we would need to make sure that all transitive functional dependencies of non prime attributes do not exist.

The professors table complies with this, as `professor_name` and `subject` are both in direct relationship with the `professor_id`. Similarly, the `student_email`, and the `student_name` are in direct relationship with `student_id`.

The grades table requires a bit more of thought. The `entry_id` is in direct relationship with `student_id` and `professor_id`. This holds well for 3NF. However, if we look at grades, though there is a direct relationship between `entry_id` --> `score`, it is also transitive, in other words `entry_id` --> `{student_id, professor_id}` --> `score`. Since the requirement for 3NF states "_all_ transitive functional dependedncies of non-prime attributes do not exist", then this table has to be changed.

A way to make this is 3NF compliant is to divide this table further. A grades table with the columns `entry_id` and `score`, and a keys table with `entry_id`, `student_id`, and `professor_id`.

4) 

There are a couple of difficulties linked with normalization. It can be difficult to design, that is, it requires some detail and thought upfront before designing the database. This also may introduce some issues if the database is to changed or scale in the future, introducing some complexity in the planning stage.

Another issue is that, because we are breaking up tables in order to avoid redundancy, there could be a table with very little data due to normalization. Because of this, there will be a need to use joins, which can increase the complexity of the queries themselves.

Because you are spreading out information to normalize a database, the performance can be reduced.

5)

So, for example, in the previous table of grades, professors, students, and keys that were normalized, a strategy is to perhaps, loosen the level of Normalization. For example, though 1NF data flattening is a good practice, as well as applying 2NF in order to avoid duplicating information, perhaps 3NF might be a bit strict, as the grades table already has a direct, unique entry for all key candidates in the table. That is, in this stage of normalization, the 2NF compliant grades table contains `entry_id`, `student_id`, `professor_id`, and `score` columns. Every `entry_id` contains a unique `{student_id, professor_id}` value pair, as well as unique `score` value. Yes, there is a transitive relationship too between `entry_id` and `score`. However, it's one less join we'd have to make if we only wanted to get the average, for example, of a student for a given semester. Ultimately, all information in each table, is unique, or in combination, is unique to the `entry_id`, and hence 3NF could be removed and only relax normalization to 2NF.
