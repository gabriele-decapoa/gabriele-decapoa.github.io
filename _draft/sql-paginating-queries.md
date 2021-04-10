---
layout: post
title: How to paginate queries in SQL
categories:
  - SQL
---

If you use relational database day by day, you know that each vendor implement ANSI SQL standard in different ways.
A difference is in pagination.

With _pagination_ we mean the way to retrieve data from a result set in chunked way, with a fixed width of chunk.
Each vendor implement this feature in a specifc way.

For example, if you use MySQL databases, a typical paginated query is
```SQL
SELECT *
FROM table
WHERE condition
LIMIT 10 OFFSET 10
```

The same query using Oracle RDBMS is
```SQL
SELECT *
FROM (
    SELECT *, rnum AS rownum
    FROM table
    WHERE rownum <= 20
)
WHERE rnum >10
```