---
layout: post
title: How to perform free text search in Cloudant
categories:
  - Cloudant
  - NoSQL
---

If you do not know Cloudant or you never use it, I will suggest to read [this introduction](https://www.ibm.com/cloud/cloudant) and to start to use following [this guide](https://console.bluemix.net/docs/services/Cloudant/getting-started.html#getting-started-with-cloudant). 
If you use massively Cloudant, you know all pros and cons about this NoSQL datastore.  

Sometimes, writing a query in Cloudant is a mess: the syntax, even if readable, is not so simple to use when you want to define a complex query.
Something like
```json
{
    "selector": {
        "year": {
            "$gt": 2010
        }
    },
    "fields": ["_id", "_rev", "year", "title"],
    "sort": [{"year": "asc"}],
    "limit": 10,
    "skip": 0
}
```
for SQL-skilled people means
```sql
SELECT _id, _rev, year, title
FROM table
WHERE year > 2010
ORDER BY year ASC
LIMIT 10 OFFSET 0
```
But if you want to perform a query like
```sql
SELECT _id, _rev, year, title
FROM table
WHERE year > 2010 OR (title LIKE '%ark%' AND year < 1990)
ORDER BY year ASC
LIMIT 10 OFFSET 0
```
the query in Cloudant could be very complex and not so easy to read.  
This is why you could define and use **search indices**.

A [search index](https://console.bluemix.net/docs/services/Cloudant/api/search.html#search) allows to query a Cloudant database using the well-known Lucene syntax.  
You need to define a special index, that takes three parameters (field name, data you want to index and an optional object with Lucene options) and you could start to perform query more easily!

In our example, to perform the last SQL query we need to create a search index like
```javascript
function(doc) {
    index("default", doc._id);
    if (doc.title) {
        index("title", doc.title, {"store": true, facet: true});
    }
    if (doc.year) {
        index("year", doc.year, {"store": true});
    }
   
}
```
then the query become
```
query=year:[2010 TO *] OR (title:*ark* AND year:[* TO 1990])
```