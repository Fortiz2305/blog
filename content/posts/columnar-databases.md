---
title: "Columnar Databases"
date: 2020-04-12T19:00:05+02:00
tags: ["Databases"]
draft: true
---

Columnar, or column-oriented, databases are becoming more and more popular in analytical query systems such as Business Intelligence (BI) modules.
In many companies, there are some data pipelines that extract information from several data sources, make some transformations to that data and then store it to these columnar databases. Then, we execute some analytical queries and they respond faster than traditional row-oriented databases. But... why are columnar databases faster for analytical queries? how do they differ from row-oriented databases? How do we decide which one do I need to use for a given use case?

### Row-oriented vs Column-oriented databases

In a database, data is stored and retrieved from disk, which is organized in units called *disk blocks*. For instance, when we want to read some information from the database, the database will retrieve that information reading from storage a block at a time.

The main difference between row-oriented and column-oriented databases is in the way they store data on the disk. Row-oriented solutions store the whole row in the same disk block. When using columnar storage, each disk block has values of a single column for multiple rows.

We will see it with an example: let's consider we have a database table with name, age and occupation as columns.

<img class="special-img-class" src="/images/columnar-databases/1st.png" />
