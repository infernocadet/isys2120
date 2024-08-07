# er diagrams

the relational approach to data refers to related, uniformly structured info. not duplicates, no order, each entry is a single data type. matching values in different tables indicate connections.

## relational schema

for each table, a `<TableName>`, followed by column names with type:

```
Supplier(
  SuppID: int,
  SName: string,
  Phone: string
)
```

### primary key

each table has an identifying column for each row. the values of this column must be unique among the rows - no two rows can have the same value for the identifier column.

in a schema, underline the column name.

e.g. Product (<ins>ProdID</ins>: int, Descr: string, SuppID: int)

### relational schema diagrams

pretty flexible. underline primary key, and show arrow from the referring table.

<p align="center">
    <img src="https://github.com/infernocadet/isys2120/blob/main/graphics/dia.png" width="350" height="auto">
</p>

### composite primary key

in some tables, sometimes no signle column can be the identifier (as repeat values are possible). however, a combination of columns may be suitable. for each possible instance it is not allow for two rows to match values in everyone one of the columns involved.

a combination of columns makes a composite primary key. in schema, underline all columns involved.

e.g.: SupplierCountryOffice(<ins>SuppID</ins>, <ins>CountryCode</ins>, StreedAddress)

## database design sequence

- understand - requirements analysis
  - what data is to be stored
  - what applications must be built
  - what operations are most frequent
- develop - conceptual design
  - high-level description of the data closely matching how users think of the data
  - for communication with stakeholders
- convert - logical design
  - conceptual design into a relational database schema
- convert - physical design
  - logical schemda into a physical schema (storage choices) for a specific DBMS

## conceptual data model

goal: specification off database information content.

methodology:

- conceptual design: technique for understanding and capturing business info requirements
  - depicts associations among different categories of data, shown with a diagram notation
- convert conceptual database design to logical schema and express that in SQL DDL

conceptual database design does not tell how data is implemented, created, modified, used, or deleted.

## entity relationship model

depicts associations among different categories of data in a business.

looks at:

- what are the entities and relationships?
- what information about these entities and relationships should we store in the database?
- what are the integrity constraints or business rules?

it generally describes the data to be stored.

## entities

a person, place, object or concept.

**entity type**: (aka **entity set**) is a collection of entities that share common properties.

**attribute**: describes one aspect of an enttiy type

### entity types and attributes in er diagram

entity type: represented by rectangle

attribute: depicted by ellipse. double ellipse for a multi-valued attribute (i.e., can hold multiple values), primary key columns are underlined

<p align="center">
    <img src="https://github.com/infernocadet/isys2120/blob/main/graphics/er.png" width="350" height="auto">
</p>

## relationships

relates two or more entities. e.g. John _is enrolled in_ ISYS2120

**relationship type** (or set) is a set is a set of similar relationships (the entities in each relationship are always from the same entity types). e.g. `Student` (entity type) related to `Subject` (entity type) by `EnrolledIn` (relationship type)

in an er diagram, this is represented by a diamond, labelled with the name of the relationship.

<p align="center">
    <img src="https://github.com/infernocadet/isys2120/blob/main/graphics/rel.png" width="350" height="auto">
</p>

### relationship attributes

relationships can also have additional properties:

- john enrols in isys2120 **as an elective**
- john and isys2120 are related
  - `elective` is the value of `degree_role` in this relationship

### relationship role

each participating entity can be named with a specific role

- e.g. john is value of `Student` role, ISYS2120 is the value of the `Subject` role
- explicit names for roles are useful when it comes to relationship types which may seem recursive - relates elements of the same entity type (e.g. `supervises` can be a relationship type between two `Employees`, roles can be `Manager` and `Subordinate`)

you can put the role name on the line connecting the entity to the relationship.

<p align="center">
    <img src="https://github.com/infernocadet/isys2120/blob/main/graphics/roles.png" width="350" height="auto">
</p>

- diamond represents relationship type
- ellipse represents attribute of the relationship type, linked by line to diamond
- lines connect the involved entity types to the relationship type
- roles are edges labelled with role names
  - role label is often omitted when same as entity name

<p align="center">
    <img src="https://github.com/infernocadet/isys2120/blob/main/graphics/roles1.png" width="350" height="auto">
</p>

### relationship degree

degree of relationship refers to the number of entity types involved:

- unary (recursive)
- binary (most common)
- ternary (three)

<p align="center">
    <img src="https://github.com/infernocadet/isys2120/blob/main/graphics/deg.png" width="350" height="auto">
</p>

## multiplicity of a relationship

<p align="center">
    <img src="https://github.com/infernocadet/isys2120/blob/main/graphics/mul.png" width="350" height="auto">
</p>

consider the `enrolledin` relationship of Student and Subject. students can be enrolled in zero-to-many subjects. subjects can enrol zero-to-many students. this is the default multiplicity in the ER diagram. it is a many-to-many relationship.

<p align="center">
    <img src="https://github.com/infernocadet/isys2120/blob/main/graphics/mul2.png" width="350" height="auto">
</p>

consider the `worksIn` relationship of Employee and Faculty:

- each employee can only work for max 1 department, but departments can have 0 to many employees.

hence `worksIn` is a many-to-one relationship with **total participation**

<p align="center">
    <img src="https://github.com/infernocadet/isys2120/blob/main/graphics/mul1.png" width="350" height="auto">
</p>

consider the `whereExamHeld` relationship between Subject and Room.

here, each subject can only be held in **at most 1 room**, but a room may have 0, 1 or more exams held in it. this is a many to one relationship from subject to room (many subjects can be held in 1 room)

<p align="center">
    <img src="https://github.com/infernocadet/isys2120/blob/main/graphics/mul3.png" width="350" height="auto">
</p>

consider the `CountsTo` relationship of subject and degree. subjects should count towards completion of at least 1 degree, and each degree must have at least 1 subject that counts toward it. this is a many to many relationship between subject and degree with total participation.

> [!TIP]
> to think about relationships, ask:
> to how many instances of B can a given instance of A be related?
> to how many instances of A can a given isntance of B be related?

"many to 1 from A to B" means **each A** is related to **at most 1 B**.

### key constraint on diagram

if, for a particular participant entity type, each entity participates in **at most one** relationship instance, we say that the corresponding role is a key of relationship type.

- e.g., a subject instance is in at most on WhereExamHeld relationship instance (there is at most one room where the subject has an exam)
- a many-to-one or N:1 relationship from Subject to room

representation in the ER diagram is an **arrow from the keyside rectangle to the relationship diamond**.

when it is a many to many or one to many relationship, where one entity instance can participate in many relationship instances, we just have a plain line. we have an arrowhead, when these entity instances can only participate in at most one of the relationship instances. so in this case, one or many subjects can only be held in one room. on the other hand, rooms can hold many different exams.

<p align="center">
    <img src="https://github.com/infernocadet/isys2120/blob/main/graphics/arrow.png" width="350" height="auto">
</p>

### participation constraint on diagram

now lets say, for a particular participant entity type, each entity participates in **at least one** (total participation) relationship instance, we say that the correspoding role has total participation <ins>in the relationship</ins>.

> [!NOTE]
> for example, a subject instance is in at least one CountTo relationship instance (the subject must count towards a degree, at least 1 degree.). here, subject has total participation in CountsTo.

a total relationship is shown by a thick line from the entity type that must participate to the relationship diamond.

in contrast with **partial participation**, where an entity can participate in a relationship but they might not, that is just a normal line.

<p align="center">
    <img src="https://github.com/infernocadet/isys2120/blob/main/graphics/thick.png" width="350" height="auto">
</p>

### participation and key constraint

if every entity participates in exactly one relationship, there is both a participation and key constraint hold. this is a **thick arrow**.

for example, to show `every employee works in exactly one faculty`, this means:

- every employee works in at least one faculty (total participation, thick line)
- every employee works in at most one faculty (key constraint, arrowhead)

<p align="center">
    <img src="https://github.com/infernocadet/isys2120/blob/main/graphics/both.png" width="350" height="auto">
</p>

## weak entities

an entity type that does not have a primary key among its attributes. this means, instances need more than the attributes of the entity to distinguish them.

instead, entity instances are distinguished in part by knowing which entity of another type, that they are in a total N:1 relationship with.

we call this the **identifying relationship** - and the main entity is called the **identifying entity type**.

then, among the instances related to another instance of the identifying type, we can distinguish them by an attribute (or combination of attributes) which we call the **discriminator**.

e.g.: payment of a loan, question of a quiz. their identity is that they belong to an identifying entity.

### representation of weak entity types

the situation is lets say we want to track loan repayments as a bank. as a bank, we are giving out multiple different loans. lets say we receive multiple 1st repayments for a bunch of different loans on the same day. these loan repayments just by themselves, their number (1 in this case, as being the first payment) isn't enough to really discriminate which loan they are paying off. so really, the primary key we want to use is the primary key of the loan itself, its identifying entity. we can use another attribute of the weak entity, i.e., the loan repayment number, as another attribute to identify the repayment.

in general, we depict a weak entity type through double rectangles. the identifying relationship is depicted using a double diamond. we underline the discriminator of a weak entity type wish a dashed line (close to a solid line we use for primary keys, but its not enough to be a primary key).

<p align="center">
    <img src="https://github.com/infernocadet/isys2120/blob/main/graphics/weaj.png" width="350" height="auto">
</p>

in this case, the relationship diagram for the loan repayment is as follows:

<p align="center">
    <img src="https://github.com/infernocadet/isys2120/blob/main/graphics/weak.png" width="350" height="auto">
</p>

notice also, the key and participation constraint delineated by the thick arrow. this means each payment goes towards a loan, and each payment goes towards only one loan. the paymentNumber is the discriminator of the weak entity type, in combination with the loanNumber of the owner Loan entity.

## enhanced ER model

the enhanced model supports:

- specialisation/generalisation
- abstractions (aggregation)

this introduces some object-oriented like concepts, such as subclasses and superclasses, attribute inheritance.

### generalisation/specialiation

here we want to arrange entity types in a type hierarchy. we determine entity types whose instance entities are (always) also instances of another entity type, and therefore have all the attributes of the more general type.

> [!NOTE]
> definition: two entity types `E` and `F` are in an "IS-A" relationship ("F is a E") if:
>
> 1. the set of attributes of `F` is a superset of the set of attributes of `E`, and
> 2. the entity set `F` is a subset of the entity set of `E` (each F is an E)

<p align="center">
    <img src="https://github.com/infernocadet/isys2120/blob/main/graphics/isa.png" width="350" height="auto">
</p>

one can say F is a specialisation of E (F is subclass) and E is a generalsation of F (E is superclass). e.g. Student is a subclass of Person, person is a superclass of Student - Student is a specialisation of Person, Person is a generalisation of Student.

### attribute and relationship inheritance

a specialisation inherits all attributes of the superclass. a specialisation entity type inherits all the relationship participations of its superclass.

we only show on a rectangle, the attributes and relationships which it has not inherited (those not shared with the superclass.)

<p align="center">
    <img src="https://github.com/infernocadet/isys2120/blob/main/graphics/sseg.png" width="350" height="auto">
</p>

### constraints on ISA hierarchies

we can specify _overlap_ and _covering_ constraints for ISA hierarchies:

- overlap constraints
  - disjoint
    - a higher-level entity instance can belong to at most one lower-level entity set
    - represented in ER by writing `disjoint` next to ISA triangle
  - overlapping
    - default triangle, but also state explicitly
    - a higher-level entity instance can belong to more than one lower-level entity set
- convering constraints
  - total
    - a higher-level entity instance must belong to at least one of the lower-level entity sets
    - denoted using a thick line between ISA triangle and superclass.
  - partial
    - this is the default thin line
    - a higher level entity instance need not belong to one of the lower level entity sets. i.e., not all Person instances need to be Rockstars

### aggregation

consider a ternary relationship `works-on`. suppose we want to record managers for tasks performed by an employee at a branch.

<p align="center">
    <img src="https://github.com/infernocadet/isys2120/blob/main/graphics/agg.png" width="350" height="auto">
</p>

relationship sets `works-on` and `manages` represent overlapping information. every `manages` relationship corresponds to a `works-on` relationship.

aggregation is an abstraction through which we can represent relationships as higher-level entity sets.

in this example, an employee works at a branch doing a job. branches hire employees who work a job. there is a manager to manage the branch as well as the employee. so we can make the employee branch job relationship into a single entity. each branch which hires employees and each employee in a branch is managed by a manager.

# some sql notes

## regex matches

pattern consisting of character literals or metacharacters (these specify how to process regex)

| metacharacter | meaning                                                    |
| ------------- | ---------------------------------------------------------- |
| ()            | grouping                                                   |
| \|            | alternative                                                |
| []            | character list                                             |
| .             | matches any character                                      |
| \*            | repeat preceeding pattern any number of times, including 0 |
| +             | repeat preceeding pattern one or more times                |
| ^             | start of line                                              |
| $             | end of line                                                |

```sql
select title
from UnitOfStudy
where regexp_like(uos_code, '^COMP[:digit:]{4}')
```

## date and time

a couple of sql data types:

- DATE: '2012-03-26'
- TIME: '16:12:05' - can also be accurate to nano second
- TIMESTAMP: '2012-03-26 16:12:05'
- INTERVAL: '5 DAY'

SQL constants include `CURRENT_DATE` and `CURRENT_TIME`.

main ops:

- EXTRACT(_component_ FROM `date`)
  - `EXTRACT(year FROM enroldate)`
- DATE string
  - e.g. DATE '2012-03-01'
