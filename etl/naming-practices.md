---
description: General architecture tricks to keep things easer
---

# Naming Practices

Bad names become inevitable after a certain size and scale, but some pains are avoidable.

#### Never Just "ID"

When creating a primary key for a table, it's very easy to set and forget.

```text
CREATE TABLE things (
    ID int NOT NULL PRIMARY KEY
    ,col1 varchar(255) NOT NULL
    ,col2 int
    ,col3 varchar(255) DEFAULT 'foo'
    )
```

Creating a JOIN later is easier if the primary key includes the table name, so the foreign key name can match the primary key name.  If we name the above ID `things_id` instead of just ID, we can keep track of relationships easier.

```text
CREATE TABLE stuff (
    stuff_id int NOT NULL PRIMARY KEY
    ,stuff_col1 varchar(255)
    ,stuff_col2 datetime
    ,things_id int NOT NULL
    ,FOREIGN KEY (things_id) REFERENCES things(things_id)
    ) 
```

This naming convention helps lock the relationship, particularly if multiple tables in the same category \(e.g. contact data\) will relate to the same foreign key outside of the category \(e.g. agent ID\).  

```text
FROM           things
    LEFT JOIN  stuff ON things.things_id = stuff.things_id
```

As well, it becomes easier to see interpret what an INNER or OUTER JOIN will return.  The table `things` is the owner of the `things_id` explicitly.  If two tables share a foreign key and a JOIN is desired, an analyst querying the tables will know the column is a foreign key just from the naming convention.

#### Avoid Long Names

Long, descriptive names seem like a good idea.  Sometimes, description is necessary.  A table name is not a good place for a long description.

This becomes particularly egregious for procedurally created table names.  If a database has the incoming velocity and volume to make procedurally generated names worthwhile, consider acronyms.  Otherwise, the table name becomes so large even creating an alias can be frought with danger.

The below is a fictional example of a reality that comes with procedurally created table names.  

```text
SELECT col1, col2
FROM ddo_character_npc_forgotten_realms_current_orc_orc_fighter_chief_warpriest_v2
 [...]
```

The original layers make sense:

1. DDO \(overall game\)
2. Character \(general part of database\)
3. NPC \(class of character\)
4. Forgotten Realms \(architectural split\)
5. Current \(separation from archive tables\)
6. Orc \(sub-class of character\)
7. Orc Fighter \(sub-class of sub-class\)
8. Chief \(class of Orc Fighter\)
9. Warpriest Greater v2 \(specific table stats\)

Nonetheless, the execution means this becomes difficult to untangle, and easy to mistype, leading to increased debugging time and general frustration.

#### Build a Dictionary

No matter how clear the naming convention, eventually incoming data velocity, business rule changes, and sheer entropy will cause failure.  An explicit data dictionary, complete with test data and business rule definitions, will help new data professionals get up to speed and help with future debugging.

Creating a back up of the data dictionary in the same place as the database backups is a great practice, particularly so the data dictionary can overlap with the back-up documentation.

