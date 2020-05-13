---
description: A list of ways to dramatically slow down queries
---

# Please No

A lot of performant SQL writing is avoiding issues if at all posssible.  Here are some of those issues.

#### Excessive JOINs

While potentially necessary for complex queries, reducing the number of JOINs in a process is helpful.  Potentially, pulling down two datasets then merging them in the data analysis environment will take less time. 

If multiple JOINs become necessary for common tasks, creating a custom VIEW in the database can yield great performance boosts.

#### Lack of Testing

Nearly any system will have an ability to see how long a function takes to complete.  If a function is going to be run on a significant basis, testing different ways to achieve the same results is a good use of time.

Some data products even have profilers which will optimize queries or ETL steps.  Searching for and using these tools can save analyst time and processing time.  Databricks/Spark in particular has a rich toolset to measure and minimize processing time.

#### Using Large Batches

When trying to pull a large amount of data, multiple small batches is better than a few large batches.  Joining together a large clustser of small batches seems like more hassle, but gives these advantages:

1. Smaller batches are easier to rerun if something goes wrong
2. Small batches can be run across multiple processors
3. Small batches tend to have less memory failures
4. Gaps are easier to spot in small batches \(e.g., if using daily batches, a missed day can be spotted faster than if using monthly batches\)

#### Nesting Tables

As an exaggeration of what can be done but probably should never happen:

```text
SELECT * FROM (
    SELECT *, COUNT(col1) 
    FROM (
        SELECT col1, col2, col3
        FROM table1 )
    WHERE col1 == col2
    )
WHERE col3 IS NOT NULL
```

#### DISTINCT

Distinct is helpful, but also can massively slow down a query.  Try not to use this when possible.

