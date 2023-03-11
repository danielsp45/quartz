[[Databases]]

## What is a database index
Indexes are data structures that can increase a database's efficiency in accessing tabales. Indexes are not required, the database can function properly without them, but query response time can be slower.

Every index is associated with a table and has a key, which is formed by one or more table columns. When a query need to access a table that has an index, the database can decide to use the index to retrieve recors faster.

## How does an Index work?
For example in a book index, we first search the index for the topic we want. We get the page number where that topic is found in the book, and we go to that page.

Now, imagine you have a database which has a table called persons with id, frist_name, last_name and zip_code and colummns. If you wanted to get the id of a person with a specific last_name you would have to:
```SQL
SELECT ssn
FROM persons
WHERE last_name = 'Chernosky'
```
In a table with millions of lines this would take a long time.
To get better performance we can apply aliasing.

## How to add and Index to a table
Here'se the SQL code to create such and index to the table __`persons`__ using the column `last_name`:
```SQL
CREATE INDEX 1x1_test ON persons ( last_name )
```
The syntax for index creation is simple:
+ The index name -> `ix1_test`
+ The associated table -> `persons`
+ The column to use as the index key (`last_name`)

If we would execute the query again it would look for the index instead of doing a full sequential scan.

## How and Index works internally
In SQL databases, indexes are internally organized in the form of trees. Like actual trees, database indexes have many branch bifurcations, and individual records are represented by the leaves.
This tree is created when we run the command `CREATE INDEX` statement.


# Disadvantages of database Indexes
Let's go over some of the possible downsides of using too many databases secondary indexes.

## Aditional storage
The first and perhaps the most obvious drawback of adding indexes is that they take up additional storage space. The exact amount of space dependes on the size of the table and the number of columns in the index, but it's usually a small percentage of the total size of the table. A basic index only needs to store the values of the indexed columns as well as a pointer to the row in the table.

This is important to consider if you have large datasets, as adding multiple indexes to a table can quickly use a significant amount of additional storage space.

## Slower writes
Everytime we want to add anything new to our table, a write, we also need to insert this new data into the Idexed data structure, therefore increasing the write time.

## Conclusion
Indexing is a good form of increasing query speed. But it should be well planned before hand otherwise the database will end up with a lot of Indexes draging the querying times.

# Articles
```embed
title: 'What are the disadvantages of database indexes?'
image: 'https://planetscale.com/images/blog/content/disadvantages-of-database-indexes/downsides-of-indexes-social.png'
description: 'Learn about some of the possible downsides of using database indexes and how to remove unused database indexes in MySQL.'
url: 'https://planetscale.com/blog/what-are-the-disadvantages-of-database-indexes'
```

