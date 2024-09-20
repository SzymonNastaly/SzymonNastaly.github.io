+++
title = "Working with Middle Sized CSV Files"
description = "How to quickly get all the data you need."
date = 2024-07-07
[taxonomies]
topics=["Programming"]
+++
## First Ideas
For my bachelor's thesis, I had to retrieve some data from a CSV file of around 90MB.

I first tried working with some CSV-focused tools like the 
[Rainbow CSV VSCode](https://marketplace.visualstudio.com/items?itemName=mechatroner.rainbow-csv) extension
or [CSVFiddle.io](https://csvfiddle.io/).

Both of those support SQL (or at least SQL-like) queries.
Unfortunately, both tools break at various points, especially with increasing CSV file sizes or more complex queries.

## Use SQLite
In the end, I decided to just use SQLite.
1. Create a new SQLite database: 
    ```sql
    sqlite3 temp.db
    ```
2. Setting csv mode and creating a table
    ```sql
    .mode csv tablename
    ```
3. Importing your csv into the table
    ```sql
    .import filename.csv tablename
    ```
4. Start using true SQL!
    ```sql
    select * from tablename limit 10;
    ```

## Interesting and helpful links:
* The [stackoverflow answer](https://stackoverflow.com/a/56335100) explaining the CSV to SQLite workflow
* [Consider SQLite](https://blog.wesleyac.com/posts/consider-sqlite) by Wesley Aptekar-Cassels
* [Optimal SQLite settings for Django](https://gcollazo.com/optimal-sqlite-settings-for-django/) by Giovanni Collazo