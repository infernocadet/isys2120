# sql ddl, data modification

## schema

schema must be known for relational dbms to store data. this is achieved in DDL of SQL.

schema descriptions are stored in system catalogue which can be queried.

### creating tables

`CREATE TABLE name ( list-of-columns-with-types )`

### basic datatypes

<p align="center">
    <img src="https://github.com/infernocadet/isys2120/blob/main/graphics/datatypes.png" width="350" height="auto">
</p>

### sql schema names

it would be difficult to ensure that everyone uses different names for tables, constraints etc. we want to have a separate name-space for each data owner.

one can have several different schema stored in the dbms.

## integrity constraints

an integrity constraint is a condition that must be true for any contents of the data, such as domain constraints, uniqueness constraints. ICs can be declared in the schema.

a legal instance of a relation is one where the contents satisfies every specified IC.
having integrity constraints declared explcitly in the schema is a value aspect of DBSM management.

### null value

sql-based rdbms allwos a special entry `NULL` in a column to represent facts which are not applicable or yet known.

> [!NOTE]
> a schema can explicitly prevent null values from being inputted.
> a cell with NULL is often shown empty.

#### tradeoffs of use of `NULL`

is NULL is a helpful way to deal with an unknown or not applicable attribute? instead of, e.g., using a special value such as -1.

NULL is advantageous because the system knows how to treat it specially, e.g. ave(age) or min(age) would ignore null values, but -1 would impact these.

NULL can cause complications in the definition of operations, eg can't be used in math or comparators.

### not null constraint

one domain constraint is to insist that no value in a column can be null. the value can't be unknown, it must be provided.

```sql
CREATE TABLE Instructor (
    lname VARCHAR(20) NOT NULL,
    fname VARCHAR(20) NOT NULL,
    salary INTEGER,
    birth DATE NOT NULL,
    hired DATE
);
```

### primary key constraint

primary key enforces unique and not null

multiple ways to add in a primary key:

```sql
CREATE TABLE Student (
  sid INTEGER PRIMARY KEY,
  name VARCHAR(20)
);

CREATE TABLE Student (
  sid INTEGER,
  name VARCHAR,
  PRIMARY KEY (sid)
);

CREATE TABLE Student (
  sid INTEGER,
  name VARCHAR,
  CONSTRAINT Student_PK PRIMARY KEY (SID)
);

CREATE TABLE Student (
  sid INTEGER,
  name VARCHAR
); -- then adding in primary afterwards in a different command

ALTER TABLE Student (
  ADD CONSTRAINT Student_PK PRIMARY KEY (SID)
);
```

### composite PK

in some tables, no single column is an identifier
