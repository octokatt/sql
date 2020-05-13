---
description: Returning richer context
---

# Over Partition

OVER PARTITION is an amazing way to generate summary data within the greater context of a table without needed an additional step to process the SQL query output.  

For example, a manager wants a report of their call center agents with both their average handle time \(AHT\) and their department's overall AHT.

Given a table which looks like this...

```text
ID | NAME  | DEPARTMENT | CALL_TIME
---|-------|------------|-----------
 1 | Alice | Service    | 253
 2 | Bob   | Service    | 414
 3 | Cathy | Sales      | 306
```

A query can be constructed to find the average handle time by agent using OVER PARTITION.

```text
AVERAGE(t.call_time) OVER (
    PARTITION BY t.id
    ) AS 'agent_aht'
,AVERAGE(t.call_time) OVER )
    PARTITION BY t.department
    ) AS 'department_aht'
FROM table1 t
```

This can also be used to create running totals and averages by limiting the partition using the table row.  An example would be to show the running total of an account balance over a number of different accounts. 

```text
SUM(acc.amount) OVER (
    PARTITION BY    acc.account_id
    ORDER BY        acc.datetime DESC
                    ,acc.account_id DESC
    ROWS BETWEEN    UNBOUNDED PRECEDIING --from the start
         AND        1 PRECEDING          --to this row
 )
```

These can get complex, and should be tested, but can yield a great report, especially when saved as a [stored procedure](../performance/stored-procedures.md).

