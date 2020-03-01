# Outer Joins

**Traditional Joins**

```
FROM R, S
FROM R cross join S
FROM R natural join S
FROM R join S on Condition
```

* **Dangling tuples**: With joins that require some attributes to match, <font color="blue">tuples lacking a match are left out of the results.</font>

* **Outer join** preserves dangling tuple by padding them with **NULL** in the other relation.

* Inner join(normal join) doesn't pad with **NULL**

**Outer joins**

```sql
A {LEFT|RIGHT|FULL} JOIN B ON C --Theta Outer Joins
A NATURAL {LEFT|RIGHT|FULL} JOIN B --Natural Outer Joins
```

* **Left/Right Join** preserves dangling tuples on LHS/RHS, pads with nulls on opposite side.

* **Full Join** preserves dangling tuples on BOTH sides.

![](/run/media/nicolae/727D-9E8F/Sync/UofT/2019/Winter/CSC343/notes/right_join.png)

![](/run/media/nicolae/727D-9E8F/Sync/UofT/2019/Winter/CSC343/notes/full_join.png)

## Null Values

* **Checking for null values**: `IS NULL` and `IS NOT NULL`

* In SQL we have 3 truth-values: True, False, Unknown.
  
  * Think of True = 1, False = 0, Unknown = 0.5
  
  * *Boolean operators*: AND is min, OR is max, NOT is (1-x)

### Null value Usage

* **WHERE**(and therefore **NATURAL JOINS**) ignore Null values, so if you have any rows with Null's, you ignore them.

* **Aggregation** ignores NULL. Null never contributes to a  sum, average or count, UNLESS every value is Null. In that case, count is 0. 

* **SELECT** consideres 2 null values equal. So if you do select distinct on {[a,null], [a,null]} you just get 1 null

* Set operations consider 2 null values equal

# 
