1)

a)

```
SELECT guests.first_name, guests.last_name
FROM guests
LEFT JOIN bookings ON bookings.guest_id = guests.id
WHERE bookings.guest_id IS NULL;
```
b) 
```
SELECT b.check_in, b.check_out, g.first_name, g.last_name, COUNT(*)
FROM bookings AS b
JOIN guests AS g ON g.id = b.guest_id
GROUP BY b.check_in, b.check_out, g.first_name, g.last_name
HAVING COUNT(*) = 2;
```
c)
```
SELECT b.room_id, g.first_name, g.last_name, COUNT(*)
FROM guests AS g
JOIN bookings AS b ON b.guest_id = g.id
GROUP BY b.room_id, g.first_name, g.last_name
HAVING COUNT(*) > 1;
```

d)
```
SELECT bookings.room_id, COUNT(DISTINCT guests.first_name) AS unique_guests
FROM bookings 
JOIN guests ON guests.id = bookings.guest_id
GROUP BY bookings.room_id;
```
2)

a)

**What classes/entities do we need?**

We definitely need a `students` entity, as well as a `course` entity in order to track the students in the school, as well as the courses give in the school. 

Since the question requires that we have also the grades for each student taking a class, we will need a `grades` entity as well. 

**What fields/attributes will each entity need**

A `students` entity will need the following attributes `id`, `first_name`, and `last_name`. 

The `courses` entity will need the following attributes: `id`, and `course_name`. 

The `grades` entity will need the following attributes: `id`, `course_id`, and `student_id`.

**What data types do we need to use?**

For the `students`, `id` should be an INTEGER and a PRIMARY KEY, `first_name` and `last_name` should be VARCHAR.

For `courses`, `id` should be an INTEGER and a PRIMARY KEY, `course_name` should be a VARCHAR and `credits` as an INTEGER. 

For `grades`, `id` should be an INTEGER and the PRIMARY KEY.`course_id` is a FOREIGN KEY that REFERENCES `courses.id`,and `student_id` is also a FOREIGN KEY that REFERENCES `students.id`. Both of these FOREIGN KEYS should be an INTEGER. Finally, grade is a CHAR(1), that represents the single letter grade.

**what relationships exist between entities?**

As mentioned in a previous queston in this section a), every row in `students` can be taking multiple courses. Every class the student takes will have a grade.

Conversely, every class will have multiple students, and each student in the course will have a grade.

Because of this relationship, the grades table will have a grade entry for each course for every student that is taking a course.

**How should those relationships be represented in tables?**

The `students` table should have a PRIMARY KEY to represent every student, in this case `id`. 

Similarly, the `courses` table should have a PRIMARY KEY that represents the course itself, in this case `id` within this table. 

Now, `grades`, also should have a PRIMARY KEY that represents every grade granted to a given student within a given course. We'll also call this the `id`. However, the relationship between a student and their grades is many to one, likewise, the relationship between a course and the grades is one to many. The grades table will be the intermediary table between courses and students, with the grade data found in the intermediary table. These relationships are represented via a FOREIGN KEY represented in the `grades` table.

In the `grades` table, the attribute `course_id` and `student_id` would be the FOREIGN keys that link this table with `students` and `courses`. Also these foreign key would also link `students` and `courses` together via `grades`

b) the UML Diagram is found at this link: https://drive.google.com/open?id=0B0Xx9V-Et7D2eTBVZGpRQlQ0akE


3)

a)
```
SELECT students.first_name, students.last_name 
FROM students
JOIN grades ON grades.student_id = students.id
JOIN courses ON courses.id = grades.course_id
WHERE courses.name  = 'Digital Circuits Lab';
```
b)

```
SELECT grades.grade, COUNT(*) AS amount
FROM students
JOIN grades ON grades.student_id = students.id
GROUP BY grades.grade
ORDER BY grades.grade ASC;
```

c)

```
SELECT courses.name, COUNT(grades.student_id)
FROM courses
JOIN grades ON grades.course_id = courses.id
GROUP BY courses.name;
```

d) 

```
SELECT courses.name, COUNT(grades.student_id)
FROM courses
JOIN grades ON grades.course_id = courses.id
GROUP BY courses.name
ORDER BY COUNT(grades.student_id) DESC
LIMIT 1;
```

