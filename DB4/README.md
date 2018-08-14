# Relational Algebra Exercises

In this assignment you are to write relational algebra queries over a small database, executed using our RA Workbench. Behind the scenes, the RA workbench translates relational algebra expressions into SQL queries over the database stored in SQLite. Since relational algebra symbols aren't readily available on most keyboards, RA uses a special syntax described in our [RA Relational Algebra Syntax](https://users.cs.duke.edu/~junyang/radb/start.html) guide.

We've created a small sample database to use for this assignment. It contains four relations:

    Person(name, age, gender)       // name is a key
    Frequents(name, pizzeria)       // [name,pizzeria] is a key
    Eats(name, pizza)               // [name,pizza] is a key
    Serves(pizzeria, pizza, price)  // [pizzeria,pizza] is a key

[View the database](https://lagunita.stanford.edu/c4x/DB/RA/asset/pizzadata.html). (You can also [download the schema and data](https://s3-us-west-2.amazonaws.com/prod-c2g/db/Winter2013/files/pizza.sql).)

## Q1

Find all pizzas eaten by at least one female over the age of 20.

```
\project_{pizza} \select_{age > 20 and gender='female'} (Eats \join Person);
```

## Q2

Find the names of all females who eat at least one pizza served by Straw Hat. (Note: The pizza need not be eaten at Straw Hat.)

```
\project_{name} ((\select_{pizzeria='Straw Hat'} Serves) \join (\select_{gender='female'} Person) \join Eats);
```

## Q3

Find all pizzerias that serve at least one pizza for less than $10 that either Amy or Fay (or both) eat.

```
\project_{pizzeria} (\select_{price < 10 and (name='Amy' or name='Fay')} (Serves \join Eats));
```

## Q4

Find all pizzerias that serve at least one pizza for less than $10 that both Amy and Fay eat.

```
(\project_{pizzeria} (\select_{price < 10 and name='Amy'} (Serves \join Eats)) \intersect (\project_{pizzeria} (\select_{price < 10 and name='Fay'} (Serves \join Eats))));
```