# SQL Movie-Rating Modification Exercises

## Q1

Add the reviewer Roger Ebert to your database, with an rID of 209.

```sql
INSERT INTO Reviewer VALUES (209, 'Roger Ebert');
```

## Q2

Insert 5-star ratings by James Cameron for all movies in the database. Leave the review date as NULL.

```sql
INSERT INTO Rating
SELECT (SELECT rID FROM Reviewer WHERE name = 'James Cameron'), mID, 5, NULL
FROM Movie;
```

## Q3

For all movies that have an average rating of 4 stars or higher, add 25 to the release year. (Update the existing tuples; don't insert new tuples.)

```sql
UPDATE Movie
SET year = year + 25
WHERE mID IN (
    SELECT mID
    FROM (
        SELECT mID, AVG(stars) AS avgStars
        FROM Rating
        GROUP BY mID
    )
    WHERE avgStars >= 4
);
```

## Q4

Remove all ratings where the movie's year is before 1970 or after 2000, and the rating is fewer than 4 stars.

```sql
DELETE FROM Rating
WHERE mID IN (
    SELECT mID
    FROM Movie
    WHERE year < 1970 OR year > 2000
) AND stars < 4;
```
