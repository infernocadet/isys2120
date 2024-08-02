# data and DBMS in context

> [!IMPORTANT]
> outline:
> the relational data model
>
> - terminology
> - crucial Concepts

## terminology

**data**: facts that are recorded. important for users, need to persist.

**database**: a collection of data. quite large, contains all data needed to operate an organisation.

**database management system (dbms)**: software package designed to store and manage one or more databases. allows shared access from many programs.

### importance of data

day-to-day operations of an organisation depend on accurate data. long-term planning depends on accurate data.

### importance of a database

an asset, used for historical data for enterprise strategy.

the state of the database mirrors state of enterprise. database is persistent, and shared, qualified users have access to the same data for use in a variety of activities.

## storing and using data

the usual way to build an info system is to write application programs which all use a shared collection of data, stored in a DBMS.

- for core activities in large organisations, the DBMS is a commercial one (oracle, MS SQLServer, IBM DB2 etc)
- smaller organisations or non-mission-critical parts of larger organisations use Open Source Systems such as postgreSQL or MySQL
- users run app programs which access data through the DBMS
  - programs use the DBMS to retrieve data and return it to the application
  - programs can ask DBMS to update data based on user input

### managing data

typically, data management is divided, with high-level goals ("policy" - about how data is used and stored and access) set by organisational leaders, and then settings, processes, etc ("mechanisms") determined by technical staff to achieve goals.

### data management concerns

concerns include:

- deciding what data to hold and when to remove it
  - ensuring data is not lost (by accident or attack)
  - evolution of decisions as needs change
- ensuring accuracy of the data
  - on entry or against losses
- ensuring proper use of data by allowed personnel
- ensuring efficient use of data
  - how much hardware is needed and software used

### data management decisions

the performance that users get and the security of the system is impacted by how data is stored.

there is a tradeoff between performance and perhaps security. need to consider the needs of the whole organisation before making a change to a database.

## the structure of data

the key idea in DBMS is for the database to **store descriptions of the format of the data**.

this is called the **_system catalogue_**, or **_data dictionary_**.

e.g., for each employee, keep info about:

- identifier which is integer
- name which is string of up to 30 chars

this sort of infortmation about structure or other aspects of information is called _metadata_ (data about data.)

# the relational data model

## relational DBMSs

most DBMSs today store data which follows a **relational** structure. all data is seen by users as tables of related and uniformly structured information. there are no duplicates, order is not necessarily important, each entry is simple and direct, and matching values in different tables indicate connections.

### instance

an **instance** is the contents of the databae at a single time. it holds specific values, which describes a specific situation. each update changes the instance.

### schema

the **schema** describes the structure of data in a particular database.

- what tables exist
- what the columns are called, the data type of each column

the instance at any time, must fit the pattern of the schema.

imagine the schema was like the following:

```
Supplier(SuppID: int, SName:string, Phone: string)
Product(ProdID: int, Descr: string, SuppID: int)
```

the schema can also include **integrity constraints** which restrict and narrow down the possible instances. e.g. it can make sure that each supplier has a unique and different `SuppID`.

### data design

a key task when creating an information system is to decide on the schema that will be used for the data. this is called **data design**.

it proceeds in stages:

- first produce a conceptual or semantic model
- then translate this into a relational schema
- then evaluate the schema for quality, improve if needed

## languages

**DDL**: data definition language, used to define the schema. allows us to tell the DBMS what tables exist and the structure they have, including integrity constraints.

**DML**: data manipulation language, used to access, and perform CRUD operations (create, read, update, delete).

for a relational DBMS, DML and DDL are in SQL.

### sql rundown

main command: `SELECT - FROM - WHERE`
retrieves data (rows) from one or more tables that provide an answer for an information need

joins:
we can connect data from several tables:

```sql
select ProdID
from Supplier s, Product p
where s.SuppID = p.SuppID
  AND s.SName = 'Heinz';
```

## levels of abstraction

a **View** describes how a user sees the data. logical schema defines the structure of data as it is shared among all users. physical schema describes the files and indexes used for storage on disk.

<p align="center">
    <img src="https://github.com/infernocadet/isys2120/blob/main/graphics/loa.png" width="350" height="auto">
</p>

### data independence

applications are insulated from how data is structured and stored. 

- **logical data independence**: protection from changes in logical schema - e.g. from adding in an extra column or table
- **physical data independence**: protection from changes in physical structure and location of data.

data independence is a large benefit of using a DBMS.

# DBMS vs other ways

can one have a database without a DBMS? info can be stored in files, which are accessed directly by all the programs that need to use the data. files are good at keeping data.

## file vs dbms

dbms only needs to define the central data structures once. in a file, you must do it every time.

it is easier for the dbms to handle the "how" to access the data. in a file, you have to search for everything manually.

declarative queries are:
- easy to read and maintain
- usable from different applications
- efficient evaluation due to automatic optimisation

DBMS can also ensure data integrity.

### drawbacks of file systems
- data redundancy and inconsistency
  - multiple file formats, duplication of information in different files
- difficulty in accessing data
  - need to write a new program to carry out a task
- no central authority
- integrity problems

### the dbms solution

a dbms can offer solutions to all the drawbacks of a file system, however it comes at a price in performance and money.

why does an organisation use a dbms?:
- data independence and efficient access
- reduced application development time
- capability for enhanced data integrity and security
- extract value by connecting data from different sources
- uniform data administration
- concurrent access and crash recovery