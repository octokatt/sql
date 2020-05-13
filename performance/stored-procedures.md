---
description: Making known processes faster
---

# Stored Procedures

As an important step for making routine needs faster, stored procedures \(with the right database\) can yield some great speed returns.

```text
CREATE PROCEDURE aht_report @dept varchar(255)
    AS
    SELECT agent_id, agent_name, aht
    FROM agents
    WHERE agent_dept == @dept
GO;

EXEC aht_report @dept = 'sales';
```

This requires the user to have the right permissions to be able to make a procedure, and too many procedures can start to bog down a database.  Nonetheless, this is a hugely helpful tool for running reports quickly.

