---
description: Steps description and expansion
---

# ETL Steps

At the most basic, ETL steps are:

1. Extract the data from the original source
2. Transform the data into the needed form \(also Testing\)
3. Load the transformed data into the new source \(also Logging\)

#### Extract

Depending on the source, data may come in a flat export format like a .csv, or a hierarchal form like JSON.  JSON flattening can easily be an entire page in and of itself; for now, I'm going to focus on table extract points.  In either case, Python can come to the rescue.  Here are some packages I've found helpful to connect to a database.

| Database | Python Package | Documentation Link |
| :--- | :--- | :--- |
| PostgreSQL | psycopg2 | [wiki.postgresql.org](https://wiki.postgresql.org/wiki/Psycopg2_Tutorial) |
| MySQL | mysql.connector | [pynative.com](https://pynative.com/python-mysql-select-query-to-fetch-data/) |
| MongoDB | pymongo | [mongodb.com](https://www.mongodb.com/blog/post/getting-started-with-python-and-mongodb) |

Extract steps can vary widely for different SaaS products and database structures.  The process for taking medical data from an SSIS endpoint versus dropping a .csv report from NetSuite into a network folder is very different!  Referring to the specific documentation makes a world of difference here. 

Nearly any extraction point allows automation of extraction, making extraction relatively easy to schedule during non-peak hours of database use.

#### Transform

The simplicity or complexity of transform steps will depend on requirements, but each Transform should also have a test.  These can be relatively simple, but need to cover key functions in finance, sales, and operations.

```text
-- test if new currency is added; 
-- if > 0, alert finance to add new currency handling
SELECT COUNT(currency) FROM transactions
WHERE currency NOT IN ('USD', 'EUR', 'GBP');
```

Otherwise, transform steps can range from tweaking column definitions from one datetime to another, or require querying into a stripped-down, exported database, then inserting the table output into a newly created database.  This can vary in too many ways to cover here, but version control, ETL logging, and regular back-ups are critical to maintaining and improving data quality. 

#### Load

Gratefully, the load step is normally one of the more automated steps with modern systems.  Most systems now have the ability to pick up a file from a networked location securely, making this step set-and-forget.  Some Bash scripting can handle moving the end file as needed.

```text
mv home/etl/output/20200512_cus.csv ~/network/aws-grab
echo date + ' success' >> etl.log
```

Ideally, the L in ETL should stand for both Load and Log, so meta-reporting on database health can happen.  Linux defaults to saving events into log files, but keeping a separate, specific log file can help with debugging.

A good log file can help raise a flag when Load operations starts taking longer and longer each time.  If both the process time start and time end are logged,  they can be checked against the endpoint created time for further debugging and a logic statement can send an alert if the runtime exceeds a threshold.

#### Further Notes

Depending on the source, a TEL process can work better.  If the data originator has the ability, transforming the data before extraction can significantly reduce variability and potentially collapse the Extract/Load into a single step.  

However, a TEL process must continue any testing normally done during the Transform phase, and be able to apply brakes and raise a flag if a test fails.  TEL is can be faster, but only with trust-worthy data.

