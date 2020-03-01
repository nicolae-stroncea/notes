# Schemas

* Think of a Schema the same as a database.
* **With Schemas, you can create different namespaces.**, useful to avoid name clashes. Think of it as a big pot, which contains everythihng inside of it(tables, types, etc).


## The Search Path

* See the search path: `SHOW search_path`
* Set the search path: `SET search_path TO University, public;`(set multiple paths in the search path)
* default search path is `$user, public`
	* Schema `$user` is not created, but if you create it, it's already in the search path
	* Schema `public` is created for you, and if you don't specify an alternate schema, everything is created in there by default.

## Working with a Schema

### Creating a schema
* Create your own schema: `CREATE SCHEMA University`. To refer to things inside of the schema:
```SQL
CREATE TABLE University.Student (...);
SELECT * FROM University.Student;
```
> **Note**: If you refer to a name withouts pecifying what schema it is within: **any new names you define will go to default schema (public). If you create table frindle, goes to (public.frindle)**

### Removing a Schema

* remove a schema: `DROP SCHEMA University cascade IF EXISTS`
* **cascade** means everything inside is droped(recursively)
* `IF EXISTS` is there to avoid getting error message

### Usage Pattern


Can put the following at the top of every DDL file(Data Definition Language, language used to describe database schemas, saved in a plain text format)

```SQL
DROP SCHEMA IF EXISTS University CASCADE;
CREATE SCHEMA University;
SET SEARCH_PATH TO University;
```

Here's a good workflow:

1. Create DDL file with the schema(Drop previous schema, create new schema, initialize all of your tables).
2. Create a file(SQL dump) which inserts content in the database, and import them.
3. Run queries directly in shell or by importing queries written in files.
