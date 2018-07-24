# SQL

## Basic

Database are made up of **schemas**. 

**Schemas** are groups of **tables** that are composed of **fields**.

---

## Login

Login to mysql client with username: `root` and password: `root`.

```sh
$ mysql -u root -p
Enter password: root
```

---

## Data Types

Number Data Types:

- TINYINT
- SMALLINT
- MEDIUMINT
- INT
    - UNSIGNED - only positive values.
- BIGINT
- FLOAT - approximate.
    - specify a number that contain up to 5 digits and up to 2 decimal, FLOAT(5, 2).
- DOUBLE - approximate.
- DECIMAL - exact.

String Data Types:

- CHAR - store exact number of characters.
    - CHAR(13) to specify 13 characters value only.
- VARCHAR - store up to 65,535 characters or specify max number of characters.
    - VARCHAR(64) to specify no more than 64 characters.
- TINYTEXT
- TEXT - for really big strings.
- MEDIUMTEXT
- LONGTEXT

List of values (selections):

- ENUM - allow exactly one of the possible predefined values.
    - ENUM("val1", "val2")
- SET - allow any number of predefined values.

Date and Time Data Types (provide in string):

- DATE - combination of day, month, year.
    - default format: 2018-05-13
- TIME - combination of hour, minute, second.
    - default format: 21:05:40
- DATETIME - both date and time.
    - default format: 2018-05-13 21:05:40
- TIMESTAMP - seconds since January 1, 1970.

---

## Fields

- NOT NULL
    - Field can't be null.

- UNSIGNED
    - Forbid negative numbers.

- DEFAULT \<value\>
    - Default value is no value provided.

---

## SQL examples

Create a schema:

```SQL
mysql> CREATE SCHEMA fruitstore;
```

Show how a schema was created. End with `\G` for better formatting:

```SQL
mysql> SHOW CREATE SCHEMA fruitstore \G
```

Specify the schema to use:

```SQL
mysql> USE fruitstore;
```

List all schemas in the server:

```SQL
mysql> SHOW SCHEMAS;
```

Creating tables:

```SQL
mysql> CREATE TABLE fruit(
    -> id CHAR(4) NOT NULL,
    -> name VARCHAR(255) NOT NULL,
    -> stock INT UNSIGNED NOT NULL DEFAULT 0,
    -> price FLOAT UNSIGNED
    -> ) ENGINE=InnoDb;
```

```SQL
mysql> CREATE TABLE customer(
    -> id INT UNSIGNED NOT NULL,
    -> firstname VARCHAR(255)     NOT NULL,
    -> surname VARCHAR(255) NOT NULL,
    -> email VARCHAR(255) NOT NULL,
    -> type ENUM('basic', 'premium')
    -> ) ENGINE=InnoDb;

```

Disply the structure of a table:

```SQL
mysql> DESC fruit;
```

---

## Keys

Type of Keys:
- Primary Key
    - Identify unique row in a table.
    - No same value in the table.
    - Can't be `NULL`.
    - Auto increment with `UNIQUE`.
    - Example sales id.
- Unique Key
    - Field is not repeated in a table.
    - Not auto incrementable.
    - Can be `NULL`.
    - Example, user can't have same email.
- Foreign Key
    - Defines and enforce a reference between a field in a table and a row of a different table.
    - Define the key first, then foreign key reference it.
    - Example: sales id and user id.
- Index
    - Use to speed up searching the database (filtering).
    - An index can be used to efficiently find all rows matching some column in your query and then walk through only that subset of the table to find exact matches.
    - Clustered, Non-Clustered
    - Clustered - content of a phone book. (firstN, lastN)
    - Non-Clustered - many references to data in the clustered index. (location)
    - Too much indexing (**overindexing**) leads to slower query.
    - All other key types are already indexed.

---

## Altering tables

---

## Inserting rows

Inserting a single row into the table `tablename`:

```SQL
mysql> INSERT INTO tablename (field1, field2, fielde)
    -> VALUES ("1", "2", "3");
```


Inserting multiple rows into the table `tablename`:

```SQL
mysql> INSERT INTO tablename (field1, field2, field3) VALUES
    -> ("1", "2", "3"),
    -> ("4", "5", "6"),
    -> ("7", "8", "9");
```

---

## Querying Data

---

## Joining

[SQL JOIN explaination](http://www.sql-join.com/sql-join-types/)

---

## Set AUTO_INCREMENT

```SQL
mysql> ALTER TABLE tablename AUTO_INCREMENT = 4;
```

Can't set AUTO_INCREMENT to a id lower than the current highest id number.