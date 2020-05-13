---
description: The way to adjust queries quickly
---

# Creating Parameters

Creating a table within a query for reference can be a huge timesaver to be able to quickly rerun the same set of parameters, particularly over multiple sets of queries.

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

The top three lines can then be copy pasted into other queries which are set with the same WHERE clauses.  This requires good naming conventions, but allows an easy way to adjust a query without diving deeper into the code.



