# SQL Social-Network Modification Exercises

## Q1

It's time for the seniors to graduate. Remove all 12th graders from Highschooler.

```sql
DELETE FROM Highschooler
WHERE grade = 12;
```

## Q2

If two students A and B are friends, and A likes B but not vice-versa, remove the Likes tuple.

```sql
DELETE FROM Likes
WHERE ID1 IN (
    SELECT F1.ID1
    FROM Friend F1, Likes L1, Likes L2
    WHERE F1.ID1 = L1.ID1 AND F1.ID2 = L1.ID2 AND ((F1.ID2 = L2.ID1 AND L2.ID2 <> F1.ID1) OR (F1.ID2 NOT IN (SELECT ID1 FROM Likes)))
);
```

## Q3

For all cases where A is friends with B, and B is friends with C, add a new friendship for the pair A and C. Do not add duplicate friendships, friendships that already exist, or friendships with oneself. (This one is a bit challenging; congratulations if you get it right.)

```sql
INSERT INTO Friend
SELECT DISTINCT F1.ID1, F2.ID2
FROM Friend F1, Friend F2
WHERE F1.ID2 = F2.ID1 AND F1.ID1 <> F2.ID2
EXCEPT SELECT * FROM Friend;
```
