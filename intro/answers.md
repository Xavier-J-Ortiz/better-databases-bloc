
The data meanings are “number” and “value”. It seems that the data type of number is an INTEGER, the data type of value seems to be TEXT.
A database could most likely be used in an instance where there is a possibility of multiple people trying to add a new entry of data at the exact same time. Achieving this via using a text file and a programming language might generate some issues when multiple entries are simultaneous.

A Text file might be used if there is only one person at a given moment inputting one set of data at a time.


SQL allows me as a user, to focus on the searches or changes to the database that I want to make. In programming languages, I need to think about how the data is structured in a set of data, how I’m going to structure the search and how I’m going to structure the insert functions.
A database allows users to access data. The data is organized by table. Each table represents a set of meaningful columns where entries can be input. Each column within a table has a meaningful name that represents the data that will be stored in this column. These Columns can be of specific data types (TEXT, INTEGER, BOOLEAN, etc.). Data records are a set of values for each column data within the table that it is getting inserted. These records are saved, and eventually can be recovered via search query entries.
Table - is a data structure that is made up of columns that represent the data values to be inserted, and rows that make up the conceptual unit of values.

Rows - represents a single entry or set of conceptual data, made up of data values pertaining to each column,

Column - defines what each data value represents.
INTEGER, TEXT, BOOLEAN
What is the result, if we select all the dates and amounts?

5/1/2016, 1500.00
5/10/2016, 37.00
5/15/2016, 124.93 
5/23/2016, 54.72

What is the result, if we select all amounts greater than 500?

1500

What is the result, be if we select all purchases made at Mega Foods?

124.93
SELECT email, signup
FROM users
WHERE name = ‘Aleesia Algorithm’

aleesia.algorithm@uw.edu, 2014-10-24

SELECT userid
FROM users
WHERE email = ‘aleesia.algorithm@uw.edu’

1

SELECT *
FROM users
WHERE userid = 4

4, Brandy Boolean, bbolean@nasa.gov, 1999-10-15
