# Subqueries

We can use a (Subquery):

* In the **FROM clause**

* in the **WHERE**, provided it returns 1 result

Subquery must be named, so we can refer to it from outer query.



**FROM**:

```sql
SELECT sid, dept||cnum as course, grade
FROM (SELECT *
FROM Offering
WHERE instructor=‘Horton’) Hoffering
WHERE Took.oid = Hoffering.oid;
```

**WHERE**: only if subquery guaranteed to produce <font color="red">exactly</font> one value. 

```sql
SELECT sid, surname
FROM Student
WHERE cgpa >
(SELECT cgpa
FROM Student
WHERE sid = 99999);
```

> <mark>Subquery is run first</mark>, and if it returns more than 1 value, SQL returns an error.
> 
> If subquery reutrns null, then  WHERE ignores it, beacuse a >, <, =, <> null is ignored.



## Any, All, IN,EXISTS

> When a subquery can return multiple values, we can use a quantifier

```sql
--Comparisons
x comparison ANY/SOME (subquery) -- comparison holds FOR AT LEAST ONE tuple in subquery result
--Belonging to results
x comparison ALL (subquery) -- TRUE iff comparison holds FOR EVERY tuple
x IN (subquery) --TRUE iff x is in set of rows.
--Existing
EXISTS (subquery) --TRUE iff subquery has at least one tuple
NOT EXISTS (subquery) --TRUE iff subquery doesn't exist
```

## Scope

* Queries are evaluated from inside out

* If a name refers to more than one thing, use the most closely nested one

* <font color="blue">If subquery refers only to names defined inside of it, it can be evaluated once and then used repeatedly inthe query.</font>Otherwise it's evaluated once for each tuple in the outer query.


