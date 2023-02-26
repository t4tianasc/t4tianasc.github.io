---
title: "Optimizing MySQL Query Memory Usage: Reformulating Query and Creating Index"
date: 2023-02-26
categories: [databases]
tags: [sql,optimization,error]
---

# Query context
We might want to run a query as simple as this one looks:

```sql
SELECT COUNT(DISTINCT appln_nr, pub_nr) FROM patents;
```
:x: 

and get the following error:
```sql
SQL Error [5] [HY000]: Out of memory (Needed ********* bytes)
```

We are trying to select the distinct values of two integer type columns: `appln_nr` and `pub_nr`, from the table `patents` that contains around 350.000 data rows. This does not look like a query that could run out of memory.

# Solution
The first solution that we find in the web and that might seem more logical is to increase the buffer pool size, in the case of InnoDB tables, but in the case of MyISAM tables we can either:
- increase the key buffer size or
- increase the system's file cache or
- optimize the queries

In this case, we can optimize the query via **two options**:

## 1. Reformulating the query
By reformulating the query as follows:

```sql
SELECT COUNT(*) FROM (SELECT DISTINCT appln_nr, pub_nr FROM patents) b;
```
:heavy_check_mark: 
*Do not forget to name the subquery to avoid this error: `SQL Error [1248] [42000]: Every derived table must have its own alias`.*

This will return the desired result, without throwing the error mentioned above and here is why.

### Explanation
The **original query** was using a COUNT(DISTINCT) function, which makes it **create a temporary table in memory to hold the intermediate results** of the query, which in this case are all the distinct combinations of `appln_nr, pub_nr`, in order to count them. Creating a temporary table can be memory-intensive, especially if the table is large, and can lead to performance issues if there is not enough memory available.

The **reformulated query** used a subquery to select the distinct combinations of `appln_nr, pub_nr`, which means the query is **executed entirely in memory without creating a temporary table**. This is more memory-efficient than creating a temporary table and can lead to better performance.

## 2. Creating an index

```sql
CREATE INDEX index_appln_nr_pub_nr USING BTREE ON patents (appln_nr, pub_nr);
```
:heavy_check_mark:

```sql
SELECT COUNT(DISTINCT appln_nr, pub_nr) FROM patents;
```
:heavy_check_mark:

### Explanation

By creating an index on the `appln_nr` and `pub_nr` columns, the database can use the index to quickly find unique combinations of these columns without having to perform a full table scan. This can greatly reduce the memory requirements of the query and allow it to complete successfully.

***In general, creating indexes on columns that are frequently used in queries can improve query performance and reduce memory usage by allowing the database to efficiently locate the necessary data.***