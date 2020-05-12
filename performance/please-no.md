---
description: A list of ways to dramatically slow down queries
---

# Please No

A lot of performant SQL writing is avoiding issues if at all posssible.  Here are some of those issues.

#### Excessive JOINs

While potentially necessary for some complex queries, generally reducing the number of JOINs in a given process is very necessary.

If multiple JOINs become necessary for common tasks, creating a custom VIEW can yield great performance boosts, and the same goes for creating stored procedures.

#### Lack of Testing

Nearly any system will have an ability to see how long a function takes to complete.  If a function is going to be run on a significant basis, running testing on different ways to solve the same problem can shave down the runtime.

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

#### Ignoring Profilers

We live in a wonderful time where usually either the database endpoint or the SQL querying program may have a profiler which can optimize your query or ETL.  Whenever possible, use these programs.

