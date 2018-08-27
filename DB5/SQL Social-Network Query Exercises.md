# SQL Social-Network Query Exercises

Students at your hometown high school have decided to organize their social network using databases. So far, they have collected information about sixteen students in four grades, 9-12. Here's the schema:

Highschooler ( ID, name, grade )
English: There is a high school student with unique *ID* and a given *first name* in a certain *grade*.

Friend ( ID1, ID2 )
English: The student with *ID1* is friends with the student with *ID2*. Friendship is mutual, so if (123, 456) is in the Friend table, so is (456, 123).

Likes ( ID1, ID2 )
English: The student with *ID1* likes the student with *ID2*. Liking someone is not necessarily mutual, so if (123, 456) is in the Likes table, there is no guarantee that (456, 123) is also present.

Your queries will run over a small data set conforming to the schema. [View the database](https://lagunita.stanford.edu/c4x/DB/SQL/asset/socialdata.html). (You can also [download the schema and data](https://s3-us-west-2.amazonaws.com/prod-c2g/db/Winter2013/files/social.sql).)

For your convenience, here is a graph showing the various connections between the students in our database. 9th graders are blue, 10th graders are green, 11th graders are yellow, and 12th graders are purple. Undirected black edges indicate friendships, and directed red edges indicate that one student likes another student.

![graph of students](https://lagunita.stanford.edu/assets/courseware/v1/db2bd56806a9ee925f626a01600a97f9/c4x/DB/SQL/asset/social.png)

```
$ sqlite3
sqlite> .read social.sql
```

## Q1

Find the names of all students who are friends with someone named Gabriel.

```sql
SELECT name
FROM Highschooler
WHERE ID IN (SELECT Friend.ID1
             FROM Highschooler JOIN Friend
             WHERE Highschooler.ID = Friend.ID2 AND Highschooler.name = 'Gabriel');
```

## Q2

For every student who likes someone 2 or more grades younger than themselves, return that student's name and grade, and the name and grade of the student they like.

```sql
SELECT S1.name, S1.grade, S2.name, S2.grade
FROM (
    (
        SELECT id, name, grade
        FROM Highschooler JOIN Likes
        WHERE Highschooler.ID = Likes.ID1
    ) S1 JOIN (
        SELECT Likes.ID1 AS id, name, grade
        FROM Highschooler JOIN Likes
        WHERE Highschooler.ID = Likes.ID2
    ) S2 USING(id)
)
WHERE s1.grade - s2.grade > 1;
```
