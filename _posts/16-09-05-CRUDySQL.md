---
title: Grease II and other CRUD'y SeQueLs
excerpt: "Databases, SQL and awful puns."
teaser: data.png
header:
  overlay_image: grease.jpg
  overlay_filter: 0.5
---
## Introduction

Before we start lets agree on one thing. Grease II was an absolute train wreck. If you disagree, the little red button in the corner can be used to raise your objections.

Now on to the matter at hand. This is going to be a short post exploring the CRUD functions of databases and how we can use SQL to carry them out.

If those acronyms mean nothing to you, fear not. It's nothing a good example can't explain!

## Databases

Lets start with databases. A database is simply an organised store of data with some sort of persistence. By persistence we mean that the data is stored even after the program stops running.

There are two main types of databases; relational and non-relational. For our purposes we'll stick to relational databases today. Non-relational databases are really interesting though, and I recommend you have a look into them!

So relational databases. The mental model I've always used is nothing more than a collection of big excel spreadsheets. The individual spreadsheets are called tables. In these, we have rows that contain entries, and columns that hold the different variables we associate with each entry. Relational databases are really good for storing data which is heavily related to each other.

Before we continue, a note on Database Management Systems. This is the system that holds the database and lets you interact with it. I've used PostgreSQL for the following. However, there are quite a few different systems you can use though including MySQL and SQLite. This [blog](https://www.digitalocean.com/community/tutorials/sqlite-vs-mysql-vs-postgresql-a-comparison-of-relational-database-management-systems) does a good job explaining the differences.

Lets look an an example of a database table used to hold student data. It will have names, ages, year group and the last exam grade the student received.

```
id  |  name  | age | year | grade
----+--------+-----+------+-------
  1 | Leah   |  14 |    9 | B-
  2 | Tom    |  17 |   12 | A+
  3 | Sophie |  14 |    9 | C
```

The relational structure of this database should make sense. It's just a table at the end of the day; nothing too fancy!

## SQL - The Sequel...

Alright, so we've looked at a databases and tables. But what about SQL? You never told us what it stood for. Well SQL stands for Structured Query Language. It's the language we use for working with these sorts of databases. It's very old (first seen in the 1970's), a little difficult to use, and can be very cranky. But when it works, it makes using data a pleasure.

I've always thought the best way to learn a new language is to jump straight in, so lets look now at building our students database.

## Woodworking 101 - Building your first table

Ok so, we've installed PostgreSQL, using `brew install Postgres`. Next we need to run a couple of commands in the terminal to configure our environment.

```
ln -sfv /usr/local/opt/postgresql/*.plist ~/Library/LaunchAgents
launchctl load ~/Library/LaunchAgents/homebrew.mxcl.postgresql.plist
```

Once we've done this, we should be able to open up Postgres using psql. This is the command line interface that allows us to interface with our database.

Our first step is to build a database called students. The command to build a new database is quite simple:

```SQL
# CREATE DATABASE students;
```

The semi-colon is an important feature of the SQL language. All commands need to end with one. You'll end up doing something strange if you miss one.

Alright so we have a database. Next we need to connect to that database. Postgres achieves this with a simple `\c students` command:

```
Tom=# \c students
You are now connected to database "students" as user "Tom".
```

Now we're ready to make our first table. In SQL we use the `CREATE TABLE` command to create tables. Make sense right? We first tell this command the name of the table, (STUDENTS below), and then tell it the names of the fields we want:
- An ID field which is an integer (INT), and is also a Primary Key.   
- A Name field which is a text field (TEXT).  
- An Age field which is an integer.   
- A Year field which is an integer.  
- A Grade field which is a text field.  

```
students=# CREATE TABLE STUDENTS(ID INT PRIMARY KEY, NAME TEXT, AGE INT, YEAR INT, GRADE TEXT);
```

In databases, a primary key is a unique identifier which we assign to each data entry. It must be unique and cannot be null.

```
id | name | age | year | grade
----+------+-----+------+-------
(0 rows)
```

This command creates our table, however we have no data in it. It's just an empty shell. To add data we need to turn to our other acronym. I'm sorry, but this blog is about to turn a little CRUD'y.

## CRUD - Create, Read, Update and Destroy

The CRUD acronym stands for Create, Read, Update and Destroy. These four verbs represent the most common things we do to data in a table. Lets talk about them in turn, and look at an example in SQL.

### Create - Add new entries

When we want to add new entries to our table we use the create functionality. In SQL this is provided by the `INSERT INTO` command:

```
INSERT INTO students (ID, NAME, AGE, YEAR, GRADE)   
VALUES (1, 'Tom', 17, 12, 'A+');

INSERT INTO students (ID, NAME, AGE, YEAR, GRADE)   
VALUES (2, 'Leah', 14, 9, 'B-');

INSERT INTO students (ID, NAME, AGE, YEAR, GRADE)   
VALUES (3, 'Sophie', 14, 9, 'C');
```

Once we've inserted our data, our table looks a little healthier:

```
id  |  name  | age | year | grade
----+--------+-----+------+-------
  1 | Leah   |  14 |    9 | B-
  2 | Tom    |  17 |   12 | A+
  3 | Sophie |  14 |    9 | C
```

As you can see, when we use the insert command, we tell it three things:
- First the table (students) we want to add the data into.   
- Secondly, the columns we want to insert data into.   
- Finally, the values we want to add to those columns.

### Read - Look at whats in the table

The next of our data operations is the read function. We can use it to extract data from our table. This is done in SQL with the `SELECT` keyword:

`SELECT * from STUDENTS;` - returns the entire table (* is a wildcard for all).  

`SELECT name from STUDENTS;` - returns just the names.  

`SELECT age, year from STUDENTS;` - returns the age and year columns.

### Update - Change an entries value

To update an entry in our database we use the `UPDATE` command. We need to be careful not to update every entry in our table, so we use a `WHERE` clause to target a specific record:

If you go back to the table before, you'll see Leah has a grade of B-. Lets see if we can't improve that:

```
students=# UPDATE students SET grade = 'A+' WHERE name = 'Leah';
UPDATE 1
students=# Select * from students;
 id |  name  | age | year | grade
----+--------+-----+------+-------
  2 | Tom    |  17 |   12 | A+
  3 | Sophie |  14 |    9 | B-
  1 | Leah   |  14 |    9 | A+
(3 rows)
```

Much better! Again we tell it the table name, which column to upgrade, and then which records to update.

### Destroy - Delete records from a table

Finally we get to the last of our data operations. Destroying records is done with the `DELETE FROM` command. Again we need to be careful to only delete those records we want:

I'm getting tired of all these 14 year olds. Lets remove them from the table:

```
students=# DELETE FROM students WHERE age = 14;
 id | name | age | year | grade
----+------+-----+------+-------
  2 | Tom  |  17 |   12 | A+
(1 row)
```
With that we've addressed the four main SQL data operations. Of course, th  ere are many other SQL commands out there. I'll leave it to you to read into this further if you are so inclined. Anyone working with data should know about the `DROP TABLE` command so as not to use it wrongly (yes, I'm speaking from personal experience!).

Otherwise joins are a  powerful tool used to connect data in different tables. However the true power of SQL is in composite functions where you combine selects, inserts and others. If you can imagine the data transformation you can probably code it!
