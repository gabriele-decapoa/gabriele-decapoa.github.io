---
layout: post
title: Elasticsearch grouping queries
---

Working with [Elasticsearch](https://www.elastic.co/products/elasticsearch) and doing some analytics on data stored in that, sometimes I had to perform some queries that in SQL are very simple, like

```sql
SELECT fieldName, COUNT(*)
FROM table
WHERE fieldName = X
GROUP BY fieldName
ORDER BY COUNT(*) DESC
```

With Elasticsearch you could obtain same results in different ways, but the more simple I found is:

```
GET table/_search?size=N
{
  "aggs":{
    "distict_field":{
      "cardinality": {
        "field": "fieldName"
      }
    }
  }
}
```

I hope this could help someone.
