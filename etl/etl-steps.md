---
description: Steps description and expansion
---

# ETL Steps

At the most basic, ETL steps are:

1. Extract the data from the original source
2. Transform the data into the needed form \(also Testing, see below\)
3. Load the transformed data into the new source

#### Extract

Depending on the source, data may come in a flat export format like a .csv, or a more complex form like JSON.  In either case, Python can come to the rescue my having a package to make up for where the extract point may not.  Here are some packages I've found helpful

| Database | Python Package | Documentation Link |
| :--- | :--- | :--- |
| PostgreSQL | psycopg2 | [wiki.postgresql.org](https://wiki.postgresql.org/wiki/Psycopg2_Tutorial) |
| MySQL | mysql.connector | [pynative.com](https://pynative.com/python-mysql-select-query-to-fetch-data/) |
| MongoDB | pymongo | [mongodb.com](https://www.mongodb.com/blog/post/getting-started-with-python-and-mongodb) |

Extract steps can vary widely for different SaaS products and database structures.  The process for taking medical data from an SSIS endpoint versus dropping a .csv report from NetSuite into a network folder is very different!  Referring to the specific documentation makes a world of difference here. 

#### Transform

The simplicity or complexity of transform steps will depend on requirements, but Transform can also stand for Testing.

Depending on the environment, testing can happen through a specialized tool, SQL commands, or Python scripts.

#### Load

Uploading to a source will also depend heavily on the endpoint.  Ideally, the L in ETL should stand for both Load and Log, so meta-reporting on database health can happen.

A good log file can help raise a flag when Load operations starts taking longer and longer each time.  Both the time start and the time end should be logged, and ideally this can then be checked against the endpoint created time for further debugging.

#### Further Notes

Depending on the source, a TEL process can work better.  If the data originator has the ability, transforming the data before extraction can significantly reduce variability and potentially collapse the Extract/Load into a single step.  

However, a TEL process must continue any testing normally done during the Transform phase, and be able to apply brakes and raise a flag if a test fails.  TEL is can be faster, but only with trust-worthy data.

