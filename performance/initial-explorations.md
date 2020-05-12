---
description: Keeping the total bandwidth for initial explorations low
---

# Initial Explorations

To get familiar with a new database, roaming around and looking at the data is necessary.  Unfortunately, large queries can slow down performance.  Worse, large exports of data from a new user can flag your account as fraudulent by a secops AI.  To avoid that, some of my best practices are...

#### Look at Small Data

Most of the time, you don't need a full table to get an idea of what the table is holding.  Make use of the LIMIT operation.

```text
SELECT * FROM table1 LIMIT 10;
```

Once you're relatively sure of the columns you'll need to use in the future, the next step is determining size.

```text
SELECT COUNT (*) FROM table1;
```

This confirms whether the table will be the right size to pull down, or a further trim down with a WHERE statement would be more helpful.  Especially before looking at more interesting ways to directly pull data, know the size of the data on which SQL is being performed.

#### Understand the Scope

With good business requirements, data pulls can be small and actionable.  I'm a big fan of pulling an extra month of data in each direction given a request, but if your source has multiple years of data then pulling all dates for a monthly update is overkill.

Assuming a table has a timestamp for the data involved...

```text
SELECT min(timestamp), max(timestamp) FROM table1;
```

Then, select the dates needed.

```text
SELECT col1, col2, col3 FROM table1 t
WHERE start_date < t.timestamp < end_date
```



