1) ------------------

INSERT INTO

UPDATE

DELETE FROM

2) ------------------

INSERT INTO is a command that needs a couple of things specified. The table that it’s going to be inserted in, should be specified, in this case table, as well as the desired columns that you want to insert, as well as the values of these columns. A new record will be created in this table. The structure would look something like this: 

INSERT INTO table (column_1, column_1)
VALUES (value_col_1, value_col_2);

UPDATE requires you to specify the table that is going to be changed, in this case table. The next thing that is specified are the values of the record to be changed, and the column which should be changed. This is done through SET. Then, we use WHERE to identify the specific record to update. An entry may look something like this.

UPDATE table 
SET column_1 = new_value_1, column_2 = new_value_2 
WHERE column_3 = 53;

DELETE requires the table name in which the record you want removed is contained in. Then we use WHERE to identify the record by identifying a key  value in a column.

DELETE FROM table
WHERE column_1 = 53;

3) ------------------
Bigint - can be used to ennumerate a product
Money -  can be used to track the price of a product
Text - can be used to track the lyrics of a song
Date - can be used to track when the record was created
Timestamp - can be used to track the time of day that lunch is served

4) ------------------
a) Text
b) Boolean
c) Int
d) Int

CREATE TABLE guests
(id INTEGER
first_name TEXT, 
last_name TEXT, 
confirmed BOOLEAN, 
party_of INTEGER, 
meal_count INTEGER);

ALTER TABLE guests 
ADD COLUMN thank_you_sent BOOLEAN;

ALTER TABLE guests 
DROP COLUMN meal_count;

ALTER TABLE guests
ADD COLUMN table_number INTEGER;

DROP TABLE guests;

5) ------------------
CREATE TABLE books 
(isbn INTEGER, 
title TEXT,
author TEXT,
genre TEXT,
publish_date DATE,
copies INTEGER,
available_copies INTEGER);

INSERT INTO books 
VALUES(1, 
“Moby Dick”, 
“Herman Melville”, 
“Novel”, 
1851-10-18, 
10, 
5)

INSERT INTO books 
VALUES(2, 
“Casino Royale”, 
“Ian Fleming”, 
“Fiction”, 
1953-04-13, 
4, 
2)

INSERT INTO books 
VALUES(3, 
“Pride and Prejudice”, 
“Jane Austen”, 
“Novel”, 
1813-01-28,
 15, 
9)

UPDATE books 
SET available_copies = 8 
WHERE isbn = 3;

DELETE FROM books
WHERE isbn = 1; 

6) ------------------
CREATE TABLE spacecraft 
(id INTEGER,
name TEXT,
year_launched INTEGER,
country TEXT,
mission_summary TEXT,
orbits TEXT,
in_service BOOLEAN,
distance_from_earth INTEGER);

INSERT INTO spacecraft 
VALUES (1, 
“potato_boy”, 
1984, 
“Latvia”, 
“Mission from politiburo to search for place warmer and with more light to grow potato”, 
“Moon”, 
TRUE,
100000);

INSERT INTO spacecraft 
VALUES (2, 
“Red Hot”, 
1981, 
“Poland”, 
“Test craft for first man on the sun”, 
“Sun”, 
TRUE,
500000);

INSERT INTO spacecraft 
VALUES(3, 
“X_45”, 
2017, 
“USA”, 
“Colonize for Musk”, 
“Mars”, 
TRUE,
23452345163);

DELETE FROM spacecraft 
WHERE id = 2;

UPDATE  spacecraft 
SET in_service = FALSE 
WHERE id = 1;

7) ------------------
CREATE TABLE inbox 
(id INTEGER,
subject TEXT,
sender TEXT,
recipients TEXT,
body TEXT,
datetime_sent TIMESTAMP,
read BOOLEAN,
chain_id INTEGER);

INSERT INTO inbox
VALUES(1,
“Invitation”,
“ed@singer.com”,
NULL,
“Come to the party! Lets have fun!”,
2017-10-10 21:30:00,
FALSE,
1);

INSERT INTO inbox 
VALUES(2,
“Resignation”,
“spicer@senortadpole.com”,
“mooch@wh.org”,
“I’m out”,
2017-6-04 09:30:00,
TRUE,
2);

INSERT INTO inbox
VALUES(3,
“Celebration”,
“louie@ck.com”,
“chris@rock.com”,
“Let’s grab some icecream!”,
2017-3-02 04:00:00,
TRUE,
3);

DELETE FROM inbox 
WHERE id = 3;

UPDATE inbox
SET sender = “sandler@adam.com” 
WHERE id = 3;
