#  SQL Movie-Rating Query Exercises

You've started a new movie-rating website, and you've been collecting data on reviewers' ratings of various movies. There's not much data yet, but you can still try out some interesting queries. Here's the schema:

Movie ( mID, title, year, director )
English: There is a movie with ID number mID, a title, a release year, and a director.

Reviewer ( rID, name )
English: The reviewer with ID number rID has a certain name.

Rating ( rID, mID, stars, ratingDate )
English: The reviewer rID gave the movie mID a number of stars rating (1-5) on a certain ratingDate.

Your queries will run over a small data set conforming to the schema. [View the database](https://lagunita.stanford.edu/c4x/DB/SQL/asset/moviedata.html). (You can also [download the schema and data](https://s3-us-west-2.amazonaws.com/prod-c2g/db/Winter2013/files/rating.sql).)

## Q1

Find the titles of all movies directed by Steven Spielberg.

```sql
SELECT title
FROM Movie
WHERE director = 'Steven Spielberg';
```

## Q2

Find all years that have a movie that received a rating of 4 or 5, and sort them in increasing order.

```sql
SELECT DISTINCT year
FROM Movie JOIN Rating USING(mID)
WHERE stars = 4 OR stars = 5
ORDER BY year;
```