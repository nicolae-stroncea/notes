# Data Manipulation Language(DML)

> DDL: used for defining schemas
> DML: used for writing queries and modifying databases

## Order of Execution

1. **FROM clause & Joins** determine initial table. 
2. **WHERE** <mark>filters rows **individually**</mark> according to data FROM 1 row satisfying a condition.
3. **GROUP BY** combines rows after *WHERE* **into groups**
4. **Having** filters the groups. Goes through the Groups and just like where filters rows, it filters groups according to each group satisfying a condition.
5. **ORDER BY** arranges remaining rows/groups
6. **TOP** only chooses N amount of rows.
7. **SELECT** selects only the right columns.

## Basic queries

* Renaming tables:

> `employee e` renames table employee to e for duration of query
> 
> ```SQL
> --table e renames table to e.
> SELECT e.name, d.name
> FROM employee e, department d
> WHERE d.name = ‘marketing’
> AND e.name = ‘Horton’;
> ```
> 
> * Renaming attributes:

> Use: `attribute AS new_name`
> 
> ```SQL
> select name AS title, dept FROM course;
> ```

* Order relations: add as **final clause** `ORDER BY (attribute_list) [DESC/ASC]`
  
  * Ordering can **include expressions**: `ORDER BY sales+rentals`
  * Ordering is last thing done before SELECT, so all attributes are still available.

* SELECT with expressions:

```SQL
SELECT sid, grade+10 AS adjusted FROM Took;
SELECT dept||cnum FROM course;--Notice how every value in breadthRequirement column will be 'satisfies'
SELECT dept, 'satisfies' AS breadthRequirement FROM Course WHERE breadth;
```

* Comparing string to a pattern:
  * Patterns are: %(means any string, of **0+**length), _(any **single character**)
  * Comparisons are done with: `attribute LIKE '%pattern_'` and attribute NOT LIKE '%pattern_'

```SQL
SELECT * FROM COURSE
WHERE name LIKE '%Comp%';
```

## Aggregation and Grouping

> These include: SUM, AVG, COUNT, MIN, MAX, which can be applied to a column in a SELECT clause. When they are applied, they will only create 1 ROW, since by default they will take the aggregate function across the entire table.

* If we have a SELECT-FROM-WHERE with GROUP BY, tuples are grouped according to values of those attributes AND any aggregation gives us a **single value per group**
* **SELECT**: Each element of SELECT must be either  an aggregate function(SUM, MAX, MIN, COUN), or an attribute on the GROUP BY list. 
* **HAVING**: similar to select, HAVING can refer to attributes ONLY IF they're aggregate functions OR attributes of GROUP BY list.

## Union, Intersection and Difference

```sql
(subquery) UNION/INTERSECT/EXCEPT/UNION ALL (subquery) --brackets are mandatory
(SELECT sid FROM Took WHERE grade > 95) UNION (SELECT sid FROM Took WHERE grade < 50)
```

![](/home/nicolae/Winter/CSC343/notes/bag_sets.png)

> Union ALL does not eliminate duplicates, but Union does.
> 
> **EXCEPT returns the distinct rows that do not appear in a second result set.****

### Controlling Duplicate Elimination

* We remove duplicate rows with **SELECT DISTINCT**

* We force to keep duplicates by doing **UNION ALL**

### Views

* A view is a relation(table) defined in terms of stored table(**base tables**) +  other views.

* You can access a view like a base table.

There are 2 types of views:

1. **Virtual**: no tuples are stored, view is just a query constructed dynamically when needed(from base tables + other views)

2. **Materialized**: constructed and stored. Problem is: <font color="red">Expensive to maintain!</font>, because you need to modify memory every time.

Views are used to:

* **Break down a large query**

* Provide **another way of looking** at the same data

> View for students who earned an 80 or higher in a CSC course.

```sql
CREATE VIEW topresults AS -- this is the main line. The rest is a normal query.
SELECT firstname, surname, cnum
FROM Student, Took, Offering
WHERE
    Student.sid = Took.sid AND
    Took.oid = Offering.oid AND
    grade >= 80 AND dept = 'CSC';
```

> We will only use virtual views. psql only started supporting materialized views recently.


