1)
The data meanings are “number” and “value”. It seems that the data type of number is an INTEGER, the data type of value seems to be TEXT.

2)
A database could most likely be used in an instance where there is a possibility of multiple people trying to add a new entry of data at the exact same time. For example, a system that keeps tracks of books in a library. This would have to track users, book quantities, book titles, user fines, and how many books are currently checked out. Achieving this via using a text file and a programming language might generate some issues when multiple entries are simultaneous.

A Text file might be used if there is only one person at a given moment inputting one set of data at a time.

3)
SQL is a declarative language, rather than a programming language which is a procedural language. In a SQL statement or query, SQL is in charge of doing what we are asking it to do, and will output or do what we asked it to do. A programming language, you have to define the whole process and provide steps on how achieve what you want, defining along the way how the process will be served. In a declarative language, you set the command or order, and let the system deal with how it's completed. The result is then presented without digging into how it should be done.

4)
A database allows users to access data. The database is typically a collection of tables that relate to each other via a foreign key. Relevant data is organized within these tables. Each table represents a set of meaningful columns where entries can be input. Each column within a table has a meaningful name that represents the data that will be stored in this column. These Columns can be of specific data types (TEXT, INTEGER, BOOLEAN, etc.). Data records are a set of values for each column data within the table that it is getting inserted. These records are saved, and eventually can be recovered via search query entries.

5)
Table - is a data structure that is made up of columns that represent the data values to be inserted, and rows. Rows are a record of data, and represents a single, implicitly strucutred data item in a table.

Rows - represents a single entry or set of conceptual data, made up of data values pertaining to each column.

Column - defines what each data value represents.

Value - is a cell within a database table found at the intersection of a row and a column. It's a distinct piece of data within a row, that is assigned to that row's column.

6)
INTEGER, TEXT, BOOLEAN
7)
What is the result, if we select all the dates and amounts?

5/1/2016, 1500.00
5/10/2016, 37.00
5/15/2016, 124.93 
5/23/2016, 54.72

What is the result, if we select all amounts greater than 500?

1500

What is the result, be if we select all purchases made at Mega Foods?

124.93

8)
query:

SELECT email, signup
FROM users
WHERE name = ‘Aleesia Algorithm’

result:

aleesia.algorithm@uw.edu, 2014-10-24

query:

SELECT userid
FROM users
WHERE email = ‘aleesia.algorithm@uw.edu’

result:

1

query:

SELECT *
FROM users
WHERE userid = 4
 
result:

4, Brandy Boolean, bbolean@nasa.gov, 1999-10-15
