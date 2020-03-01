# Data Definition Language(DDL)


## Types

* Each column must have a type. List of possible types:
	* Char(n): **fixed-length string** of n characters, padded with blanks if necessary
	* Varchar(n): **variable-length** string of up to n characters
	* Text: **variable-length, unlimited**. PSQL and other implementations support it, but it's not in SQL standard.
	* Int: just integer
	* Float: Real number(+/-, decimal places, etc) e.g: *1.49, 37.96e2*
	* Boolean e.g: *TRUE, FALSE*
	* Time-related:
		* **Date**: just the date, no time in hrs, minuts, e.g: ‘2011-09-22’
		* **Time**: just the time in hrs, minutes, etc, no Date, e.g: '15:00:02.5'
		* **TimeStamp**: you know both the time + the date, e.g: 'Jan-12-2011 10:25'


### User-defined types

* Can define your own types, where you can **set constraints**:
```SQL
create domain Grade as int
	default null
	check (value >= 0 and value <= 100);
create domain Campus as varchar(4)
	default 'StG'
	check (value in ('StG','UTM','UTSC'));
```
* `domain` is a data type with optional constraints
* constraints are checked every time new value is added. 

## Constraints

* Primary key, foreign key, checks, unique, they're all constraints.
* Can give them a name(except for primary key) by adding `constraint <<name>>` before `check (<<condition>>)`
```SQL
constraint gradeInRange check (value >= 0);
constraint validCourseReference foreign key (cNum, dept) references Course;
```

### Check Constraints

* A check constraint is the most generic constraint type. It allows you to specify that the value in a certain column must satisfy a Boolean (truth-value) expression.
* A check constraint is satisfied if the check expression evaluates to true or the null value. Since most expressions will evaluate to the null value if any operand is null, they will not prevent null values in the constrained columns
* Check constraint can check across the column, OR across the entire row(**in PSQL**). It can also check across the entire table(**not in PSQL**).

```SQL
# this one gives the check a name
CREATE TABLE products (
    product_no integer,
    name text,
    price numeric CONSTRAINT positive_price CHECK (price > 0)
);
# this one does a general check on the row.
CREATE TABLE products (
    product_no integer,
    name text,
    price numeric CHECK (price > 0),
    discounted_price numeric CHECK (discounted_price > 0),
    CHECK (price > discounted_price)
);
```

### Not null Constraints
A not-null constraint simply specifies that a column must not assume the null value. A syntax example:
```SQL
# must always be written as column constraint
CREATE TABLE products (
    product_no integer NOT NULL,
    name text NULL,
    price numeric NOT NULL CHECK (price > 0)
);

### Unique

* same format as primary key declaration
* set of attributes form a key(they are unique)
* 1 value + can be null(or all of them). two null values are never considered equal in this comparison(you can think of NULL as unknown). That means even in the presence of a unique constraint it is possible to store duplicate rows that contain a null value in at least one of the constrained columns. 
* Can be written as a column constraint or as a tqable constraint.
* `UNIQUE NOT NULL` has same meaning as `Primary Key` except you can have as manu `unique not null` as you want but you can only have 1 `Primary Key`
```SQL
CREATE TABLE products (
    product_no integer UNIQUE,
    name text,
    price numeric
);

# this specifies that the COMBINATION of prodcut_no and name MUST BE UNIQUE
CREATE TABLE products (
    product_no integer,
    name text,
    price numeric,
    UNIQUE (product_no, name)
);
```
### Primary Key

* Set of attributes forms a key(**unique**)[link](#-userdefined-types)
* Values are**never null**(<mark>each one of the attributes is NOT NULL</mark>)
* Very useful for **optimizations**

Option 1 is if you only have **1 primary key**

```SQL
create table Blah(
	age1 integer primary key;
	age2 integer;
	age3 integer
);
```

Option 2 is if you want **1+ primary keys**: add pk at the end of table definition(still inside):

```SQL
create table Blah(
	age1 integer;
	age2 integer;
	age3 integer;
	primary key (age1, age2);
)
```


### Foreign Key

`foreign key (SID) references Student`

1. Every value of sID in this table **must occur** in Student table
2. <mark>Must be declared either primary key or unique in Student table</mark>

Has same syntax as unique and Primary Key

```SQL
create table People (
SIN integer primary key,
name text,
OHIP text unique);

create table Volunteers (
email text primary key,
OHIPnum text references People(OHIP));
age int check (age > 0);
```

### Assertions

* Assertions can express ross-table constraints: `create assertion (name) check (predicate);`
* Expensive because have to be checked on every database updated
* Assertions are not supported by PostreSQL(or most other DBMS's)

### Triggers

* Triggers are compromise between checks AND Assertions
	* They are powerful
	* You control the cost by deciding when they are applied.

#### Process

1. Specify type of database even you want to respond to: (after delete on Courses, before update of grade on Took, etc)
2. Specify the response: (insert into Winners values (sID)) as a function.

**Creating the function**:

```SQL
create function RecordWinner() returns trigger
as
$$
begin
	if new.grade >= 85 then
		insert into Winners values (new.sid);
	end if;
	return new;
end
$$
language plpgsql
```

**The trigger**:

```SQL
create trigger TookUpdate
before insert on Took #specify the event
for each row
execute procedure RecordWinner(); # specify what function you want to u
```

### Reaction Policies

* Assume table R refers to table S
* You can define "fixes" propagate backwards from S to R. *We define them in R because that's the table that will be affected*
* **Cannot** define "fixes" that propagate  forward from R to S.

#### Events

You can react to:

* on delete(when row deleted in S creates *dangling* reference)
* on update(when an update in S creates *dangling* reference(i.e maybe one of values of p.k is changed))
* or both: `on delete restrict on update cascade`

#### reactions

* `restrict`: don't allow the deletion/update in S. Therefore, if I delete a row from S which has a reference in R, DBMS won't let me.W
* `cascade`: make the same deletion/update in the referring tuple. if I delete row from S, also delete all rows from R which reference it.
* `set null`: set *corresponding value*(not entire tuple!!) in referring tuple to null.


## Semantics of Deletion

Deletion proceeds in 2 stages:

1. Marik all tuples for which the WHERE condition is satisfied
2. Go back and delete the markwed tuples.


## Updating Schema itself

* Alter: alter a domain or table
```SQL
alter table Course
	add column numSections integer;
alter table Course
	drop column breadth;
```
* Drop: remove a domain, table, or whole schema
```SQL
drop table course;
```
* `delete from course` removes all referring rows.













