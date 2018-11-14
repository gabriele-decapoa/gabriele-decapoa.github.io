---
layout: post
title: How to limit Elasticsearch grouping queries
categories:
  - Elasticsearch
  - grouping
---

In [another post](https://gabriele-decapoa.github.io/2017/09/16/Elasticsearch-grouping-queries/) I show how to convert SQL queries like

```sql
SELECT fieldName, COUNT(*)
FROM table
WHERE fieldName = X
GROUP BY fieldName
ORDER BY COUNT(*) DESC
```
into an Elasticsearch query. 
But what about limiting the number of results?   

In SQL (ANSI version) this kind of query should be
```sql
SELECT fieldName, COUNT(*)
FROM table
WHERE fieldName = X
GROUP BY fieldName
ORDER BY COUNT(*) DESC
LIMIT n
```

With Elasticsearch you could obtain same results in different ways, but the more simple I found is:

```
GET table/_search?size=N
{
  "aggs":{
    "aggregation_name":{
      "terms": {
        "field": "fieldName",
        "size": n
      }
    }
  }
}
```
