---
title: "Column-oriented Databases"
date: 2020-04-12T19:00:05+02:00
tags: ["Databases"]
---

Columnar, or column-oriented, databases are becoming more and more popular in analytical query systems such as Business Intelligence (BI) modules.
In many companies, it's common to see some data pipelines that extract information from several data sources, make some transformations to that data and then store it to these columnar databases. Then, we execute some analytical queries and they respond faster than traditional row-oriented ones. But:

  * What is a column-oriented database? how they differ from row-oriented databases?
  * Why are they faster for analytical queries? Are they a silver bullet for all use cases?

### Row-oriented vs. Column-oriented databases

In a database, data is stored and retrieved from disk, which is organized in units called *disk blocks*.

<img src="/images/columnar-databases/database-disk.png"/>

**The main difference between row-oriented and column-oriented databases is in the way they store data on the disk**. Row-oriented solutions store the whole row in the same disk block, while columnar databases stores data by column, rather than my row. So, in column-oriented databases each disk block has values of a single column for multiple rows.

Let's see it with an example. Consider we have the database table below, with three records and three columns (name, age and occupation).

<img src="/images/columnar-databases/database-table.png" />

The picture below shows how records are stored into disk blocks when using a row-oriented database.

<img src="/images/columnar-databases/row-database.png" />

We can see that the first disk block contains all the table columns for Lisa, the person in the first row. Then, the second blocks contains all the columns for the second row, etc. So, in this solution, **each disk block contains all the column values until complete the entire row**. Some examples of row-oriented databases:

  * MySQL
  * PostgreSQL
  * Amazon Aurora

Let's look at the same scenario in a column-oriented database:

<img src="/images/columnar-databases/column-database.png" />

In this case, the values for each column are stored sequentially into disk blocks. The first block contains the *Name* information for the three rows, the second one contains all the *Age* information, etc. So, **when using a columnar database, each disk block contains values of a single column for multiple rows**. Some examples of column-oriented databases:

  * Amazon Redshift
  * Google BigQuery


### Why are columnar databases so popular for business intelligence (BI) applications?

To respond this question, we will firstly dive into the two types of database systems.

* **OLTP** (**O**n**l**ine **T**ransactional **P**rocessing) systems work pretty well when looking up, inserting, updating or deleting **specific** information. In our example, some good use cases for OLTP systems would be:
  * What is the occupation of a person with name Lisa and age 39?
  * What is the name of the person with age 27 who work as a doctor?
  * Update the age of entries with age 39 to 40

* **OLAP** (**O**n**l**ine **A**nalytical **P**rocessing) systems excel at queries that require **large table scans**. We want to process large datasets and answer questions about the processed data. Going to our example, some good tasks for OLAP systems can be:
  * What's the average age of the people in our application?
  * What is the most common occupation in our system?

We are going to analyze the use case of getting the average age of people in our table (Fig. 2) in both row-oriented and column-oriented solutions. If we put it as a SQL query, it would be something like:

```
SELECT AVG(Age) FROM table_name
```

When we execute that query, databases read data from storage block by block. So, in a row-oriented solution, as we can see in the picture below, we have to read all three blocks from disk to get the ages of the people in our database, as each block contains a single row.

<img src="/images/columnar-databases/row-oriented-block.png" />

If we execute the same query in a column-oriented database, **we now have to read one block from disk**, as all the ages are in the same block.

<img src="/images/columnar-databases/column-oriented-block.png" />

This is a simple example, we cannot show a huge performance difference between the different solutions but, as the number of rows and columns increase, the amount of data to be read from disk can be significantly smaller in the column-oriented solution. Also, as columnar databases keep homogenous data in a single block, they can apply strategies to compress the data in that block. This is also an important point when you have huge volumes of data.

As business intelligence applications usually perform this type of task, this is the reason of why columnar databases are so popular for this use case.

### Are column-oriented databases the best solution for all my problems?

From [Fred Brooks's "No Silver Bullet" essay](https://people.eecs.ku.edu/~saiedian/Teaching/Sp08/816/Papers/Background-Papers/no-silver-bullet.pdf): _there is no single development, in either technology or in management technique, that by itself promises even one order-of-magnitude improvement in productivity, in reliability, in simplicity._

As always, **there is no silver bullets**. When choosing between row-oriented vs. column-oriented databases, you have to know how your data looks like, how you will query it, etc. Once you have that information, the decision will be easy.

* If your most common use cases are like the examples that we saw in the OLTP section, a row-oriented database will perform better than a column-oriented one.
* If you are working on an application which is mostly used for analytical queries, like the example we analyzed, you should give a try to a column-oriented database.
