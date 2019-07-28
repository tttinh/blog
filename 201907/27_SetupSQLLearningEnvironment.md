# Setup SQL Learning Environment.

[Home](../README.md)

Recently, one of my college ask me a question about SQL query. To be honest, I'm not familiar with SQL (only use simple queries), and hardly uses it in my everyday job. But I think it's a necessary skill, so I plan to learn it. And my goal is that I can use it in a comportable way (No need to Google every queries).

## Learning resources

There are many materials to learn SQL on the Internet. I did some Google and found that we can setup a local database with SQLite and start playing with it without too much effort compare to other kind of database.

The site I choose to start is [sqlitetutorial](http://www.sqlitetutorial.net/). This is a very good site to start learning SQL in general, it has a sample database and many exercises for you to play with.

All you have to do is download its sample database, install your favorite SQL client tool and start writing SQL queries. In my case, I choose to install a [SQLite plugin](https://marketplace.visualstudio.com/items?itemName=alexcvzz.vscode-sqlite) for VS Code and start learning.

If you want to create your database or runs SQLite shell, just go to [SQLite offical sites](https://www.sqlite.org/index.html), download and install it.

## SELECT

The syntax of the SELECT statement is as follows:

```sql
SELECT DISTINCT column_list
FROM table_list
  JOIN table ON join_condition
WHERE row_filter
ORDER BY column
LIMIT count OFFSET offset
GROUP BY column
HAVING group_filter;
```

Break the full form into multiple parts, we have:

- Use ORDER BY clause to sort the result set.
- Use DISTINCT clause to query unique rows in a table.
- Use WHERE clause to filter rows in the result set.
- Use LIMIT OFFSET clauses to constrain the number of rows returned.
- Use INNER JOIN or LEFT JOIN to query data from multiple tables using join.
- Use GROUP BY to get the group rows into groups and apply aggregate function for each group.
- Use HAVING clause to filter groups.

Try to avoid using the asterisk (*) as a good habit when you use the SELECT statement.

### SELECT DISTINCT

This clause allows you to remove the duplicate rows in the result set. You put a column or a list of columns after the DISTINCT clause. If you use one column, SQLite uses that column to evaluate the duplicate. In case you use multiple columns, SQLite uses the combination of those columns to evaluate the duplicate.

```sql
SELECT DISTINCT
 city, country
FROM
 customers;
```

### ORDER BY

If you use the SELECT statement to query data from a table, the order of rows in the result set is unspecified. To sort the result set, you add the ORDER BY clause in the SELECT statement as follows:

```sql
SELECT
  column_list
FROM
  table
ORDER BY
  column_1 ASC,
  column_2 DESC;
```

The ORDER BY clause comes after the FROM clause. The ORDER BY clause allows you to sort the result set based on one or more columns in different orders: ascending and descending.

You can sort the result set using a column that does not appear in the column list of the SELECT clause.

Instead of specifying the names of columns, you can use the column’s position in the ORDER BY clause. For example, the following statement sorts the tracks by both AlbumId and Milliseconds in ascending order.

```sql
SELECT
  name,
  milliseconds,
  albumid
FROM
  tracks
ORDER BY
  3,2;
```

The number 3 and 2 refers to the `albumid` and `milliseconds` in the column list that appears in the SELECT clause.

### LIMIT

The LIMIT clause is an optional part of the SELECT statement. You use the LIMIT clause to constrain the number of rows returned by the query. For example, a SELECT statement returns one million rows. However, if you just need the first 10 rows in the result set, you add the LIMIT clause to the SELECT statement to get exact 10 rows.

```sql
SELECT
 trackId,
 name
FROM
 tracks
LIMIT 10;
```

If you want to get the first 10 rows starting from the 10th row of the result set, you use OFFSET keyword as the following:

```sql
SELECT
 trackId,
 name
FROM
 tracks
LIMIT 10 OFFSET 10;
```

You often find the uses of OFFSET in web applications for paginating result sets. You can use the ORDER BY and LIMIT clauses to get the nth highest or lowest value row. For example, you may want to know the second longest track, the third smallest track, etc.

### WHERE

The WHERE clause is an optional clause of the SELECT statement. You add a WHERE clause to the SELECT statement to filter data returned by the query. The WHERE clause is also known as a set of conditions or a predicate list.

Besides the SELECT statement, you can use the WHERE clause in the UPDATE and DELETE statements.

*Comparison operators*: tests if two expressions are the same.

| Operator        | Meaning                  |
|:----------------|:-------------------------|
| =               | Equal to                 |
| <> or !=        | Not equal to             |
| <               | Less than                |
| >               | Creater than             |
| <=              | Less than or equal to    |
| >=              | Creater than or equal to |

*Logical operators*: allow you to test the truth of some expressions. A logical operator returns 1, 0, or a NULL value.

| Operator        | Meaning                  |
|:----------------|:-------------------------|
| ALL             | returns 1 if all expressions are 1 |
| AND             | returns 1 if both expressions are 1, and 0 if one of the expressions is 0 |
| ANY             | returns 1 if any one of a set of comparisons is 1 |
| BETWEEN         | returns 1 if a value is within a range |
| EXISTS          | returns 1 if a subquery contains any rows |
| IN              | returns 1 if a value is in a list of values |
| LIKE            | returns 1 if a value matches a pattern |
| NOT             | reverses the value of other operators such as NOT EXISTS, NOT IN, NOT BETWEEN, etc |
| OR              | returns true if either expression is 1 |

#### IN

The IN operator allows you to check whether a value is in a list of comma-separated list of values. To get the tracks that belong to the artist id 12, you can combine the IN operator with a subquery as follows:

```sql
SELECT
 trackid,
 name,
 albumid
FROM
 tracks
WHERE
 albumid IN (
 SELECT
 albumid
 FROM
 albums
 WHERE
 artistid = 12
 );
```

#### BETWEEN

The BETWEEN operator is a logical operator that tests whether a value is in range of values. If the value is in the specified range, the BETWEEN operator returns true. The BETWEEN operator can be used in the WHERE clause of the SELECT, DELETE, UPDATE, and REPLACE statements.

Note that the BETWEEN operator is inclusive. It returns true when the test_expression is less than or equal to high_expression and greater than or equal to the value of low_expression

#### LIKE

Sometimes, you don’t know exactly the complete keyword that you want to query. For example, you may know that your most favorite song contains the word `elevator` but you don’t know exactly the name. To query data based on partial information, you use the LIKE operator in the WHERE clause of the SELECT statement as follows:

```sql
SELECT
  column_list
FROM
  table_name
WHERE
  column_1
LIKE
  pattern;
```

There are two ways to construct a pattern using percent sign `%` and underscore `_` wildcards:

- The percent sign `%` wildcard matches any sequence of zero or more characters.
- The underscore `_` wildcard matches any single character.

### INNER JOIN
