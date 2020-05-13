---
description: The way to adjust queries quickly
---

# Creating Parameters

Creating a filters table within a query can be a huge timesaver to quickly adjust  or reuse parameters.

For example:

```text
WITH filters AS (
    SELECT 
        CURRENT_DATE as end_date
        ,42 AS cus_id
    )
SELECT col1, col2, col3
FROM table1
WHERE      table1.datetime < (SELECT end_date FROM filters)
    AND    table1.cus_id == (SELECT cus_id FROM filters)
```

Not only can this quickly be adjusted to take a new end\_date, but the top three lines can then be copy pasted into other queries with the same WHERE clauses.  This requires some query standardization, but can lend speed for initial investigations of a known dataset.

Note, this can work differently depending on the dialect of SQL used, so check your documentation.  As well, this can make queries more susceptible to [injection attacks](https://www.w3schools.com/sql/sql_injection.asp) if outward facing.  

Nonetheless, this is fairly universal and very useful to be able to quickly run a query, then rerun with slightly different parameters.



