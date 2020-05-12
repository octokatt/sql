---
description: Returning sub-tables and richer context
---

# Over Partition

For new analysts just starting to move from Excel to SQL, one of the biggest tools to be able to keep is being able to pivot data in a useful way.

For example, say you're a new analyst making a report to show what the average call handle time is for each agent, as well as the overall average for the agent's department.  While this can be handled in Excel or Google Sheets, a straight SQL query can do the same.

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

This can also cut down on the number of ETL steps needed if the endpoint is not capable of making calculations and should ideally be spoonfed data.

