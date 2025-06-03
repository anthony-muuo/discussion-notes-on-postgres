# ROLES AND PRIVILEDGES...

-roles own database objects,, eg tables, functions etc

-roles: are like user accs or group that control who can do what in the database

-role: can be a person, a group of people like developer , an application like a website connecting to a db

-PostgreSQL uses the concept of roles to represent user accounts. It doesn’t use the concept of users like other database systems.

-When roles contain other roles, they are referred to as group roles.

### GROUP ROLES

- In PostgreSQL, a group role is a role that serves as a container for other individual roles.
- Unlike individual roles, which typically represent users, a group role is used to manage collections of roles.
- Typically, you create a role to represent a group and then grant a membership in the group role to individual roles.
- Group roles allow you to simplify permission management. Instead of granting privileges to individual roles, you can group these roles into a group, grant privileges to a group role, and all the members of that group role will inherit those privileges.

- Group roles can have individual roles or other group roles as their members. This allows you to create hierarchical structures where you can manage privileges at different levels.

-Individual roles can be members of multiple group roles. This allows for flexible assignment of permissions and roles within the database.

## why roles matter:

1. central to authentication and authorization
1. enable fine grained acess control
1. allow scalability security management by separating privileges from individuals

-have cluster- level privileges (attributes)

-granted privileges to database object

-can possibly grand privileges to other roles

## postgresql attributes

LOGIN- allows role to connect to Db
SUPERUSER- full acess to everything
CREATEROLE - can alter/create other roles
CREATEDB - can create new database
REPLICATION LOGIN - can manage streaming an replication
INHERIT - inherits priviledges from roles

## ROLES MANAGEMENT LIFECYCLE

1. create ,,
2. modify
3. grant membership
4. drop

# Introduction to PostgreSQL CREATE ROLE statement

To create a new role in a PostgreSQL server, you use the CREATE ROLE statement.

Here’s the basic syntax of the `CREATE ROLE` statement:

```sql
        CREATE ROLE role_name;
```

- In this syntax, you specify the name of the role that you want to create after the CREATE ROLE keywords.

1. When you create a role, it is valid in all databases within the database server.

For example, the following statement uses the `CREATE` ROLE statement to create a new role called anto:

```sql
CREATE ROLE anto;
```

- To retrieve all roles in the current PostgreSQL server, you can query them from the pg_roles system catalog as follows:

```sql
SELECT rolname FROM pg_roles;
```

2. Notice that the roles whose names start with pg\_ are system roles. The postgres is a superuser role created by the PostgreSQL installer.

- In psql, you can use the \du command to show all roles that you create including the postgres role in the current PostgreSQL server:

```sql
\du
```

- The output indicates that the role anto cannot log in.

- To allow the anto to log in to the PostgreSQL server, you need to add the LOGIN attribute to it.

### Role attributes

- The attributes of a role define privileges for that role, including login, superuser status, database creation, role creation, password management, and so on.

- Here’s the syntax for creating a new role with attributes.

```sql
CREATE ROLE name WITH option;
```

- In this syntax, the WITH keyword is optional. The option can be one or more attributes like `SUPERUSER`, `CREATEDB`, `CREATEROLE`, etc.

1.  ### Create login roles
    - For example, the following statement creates a role called alice that has the login privilege and an initial password:

```sql
    CREATE ROLE alice
     LOGIN
    PASSWORD 'securePass1';
```

- Note that you place the password in single quotes (').

```sql
psql -U alice
```

1.  ### Create superuser roles

- The following statement creates a role called john that has the superuser attribute.

```sql

CREATE ROLE john
SUPERUSER
LOGIN
PASSWORD 'securePass1';

```

- The superuser role has all permissions within the PostgreSQL server. Therefore, you should create the superuser role only when necessary.

- Notice that only a superuser role can create another superuser role.

1. ### Create roles with database creation permission

```sql
CREATE ROLE dba
CREATEDB
LOGIN
PASSWORD 'securePass1';
```

1. ### Create roles with a validity period

- To set a date and time after which the role’s password is no longer valid, you use the VALID UNTIL attribute:

```sql
VALID UNTIL 'timestamp'
```

- For example, the following statement creates a dev_api role with password valid until the end of 2049:

```sql
CREATE ROLE dev_api WITH
LOGIN
PASSWORD 'securePass1'
VALID UNTIL '2050-01-01';
```

- After one second tick in 2050, the password of dev_api is no longer valid.

1. ### Create roles with connection limit

- To specify the number of concurrent connections a role can make, you use the CONNECTION LIMIT attribute:

```sql
CONNECTION LIMIT connection_count
```

- The following creates a new role called api that can make 1000 concurrent connections:

```sql
CREATE ROLE api
LOGIN
PASSWORD 'securePass1'
CONNECTION LIMIT 1000;
```

## Summary

- PostgreSQL uses roles to represent user accounts. A role that can log in is equivalent to a user account in other database systems.
- Use the role attributes to specify the privileges of the roles such as LOGIN allows the role to log in, CREATEDB allows the role to create a new database, SUPERUSER allows the role to have all privileges.

# Privileges

## Overview
- When an object(eg table,function) is created, it is assigned an owner - usually the role that executed the creation statement. For most kinds of objects, only the owner (or a superuser) can do anything with the object. To allow other roles to use it, privileges must be granted.

- There are different kinds of privileges: SELECT, INSERT, UPDATE, DELETE, TRUNCATE, REFERENCES, TRIGGER, CREATE, CONNECT, TEMPORARY, EXECUTE, USAGE, SET, ALTER SYSTEM, and MAINTAIN. 

- The privileges applicable to a particular object vary depending on the object's type (table, function, etc.). The following sections and chapters will also show you how these privileges are used.

- The right to modify or destroy an object is inherent in being the object's owner, and cannot be granted or revoked in itself. (However, like all privileges, that right can be inherited by members of the owning role)

## Assign object to new user
An object can be assigned to a new owner with an ALTER command of the appropriate kind for the object, for example

```
 ALTER TABLE table_name OWNER TO new_owner;
```

Superusers can always do this; ordinary roles can only do it if they are both the current owner of the object (or inherit the privileges of the owning role) and able to SET ROLE to the new owning role.

To assign privileges, the GRANT command is used. For example, if joe is an existing role, and accounts is an existing table, the privilege to update the table can be granted with:

```
GRANT UPDATE ON accounts TO joe;
```

Writing ALL in place of a specific privilege grants all privileges that are relevant for the object type.

The special “role” name PUBLIC can be used to grant a privilege to every role on the system. Also, “group” roles can be set up to help manage privileges when there are many users of a database

To revoke a previously-granted privilege, use the fittingly named REVOKE command:

```
REVOKE ALL ON accounts FROM PUBLIC;
```

Ordinarily, only the object's owner (or a superuser) can grant or revoke privileges on an object. However, it is possible to grant a privilege “with grant option”, which gives the recipient the right to grant it in turn to others. If the grant option is subsequently revoked then all who received the privilege from that recipient (directly or through a chain of grants) will lose the privilege. 

An object's owner can choose to revoke their own ordinary privileges, for example to make a table read-only for themselves as well as others. But owners are always treated as holding all grant options, so they can always re-grant their own privileges.


The available privileges are:
## Available Privileges

### SELECT 
Allows SELECT from any column, or specific column(s), of a table, view or other table-like object. This privilege is also needed to reference existing column values in UPDATE, DELETE, or MERGE. 

### INSERT 
Allows INSERT of a new row into a table, view, etc. Can be granted on specific column(s), in which case only those columns may be assigned to in the INSERT command (other columns will therefore receive default values). 

### UPDATE 
Allows UPDATE of any column, or specific column(s), of a table, view, etc. (In practice, any nontrivial UPDATE command will require SELECT privilege as well, since it must reference table columns to determine which rows to update, and/or to compute new values for columns.) SELECT ... FOR UPDATE and SELECT ... FOR SHARE also require this privilege on at least one column, in addition to the SELECT privilege. For sequences, this privilege allows use of the nextval and setval functions. For large objects, this privilege allows writing or truncating the object.

### DELETE 
Allows DELETE of a row from a table, view, etc. (In practice, any nontrivial DELETE command will require SELECT privilege as well, since it must reference table columns to determine which rows to delete.)

### TRUNCATE 
Allows TRUNCATE on a table.

### REFERENCES 
Allows creation of a foreign key constraint referencing a table, or specific column(s) of a table.

### TRIGGER 
Allows creation of a trigger on a table, view, etc.

### CREATE 
For databases, allows new schemas and publications to be created within the database, and allows trusted extensions to be installed within the database.

For schemas, allows new objects to be created within the schema. To rename an existing object, you must own the object and have this privilege for the containing schema.

For tablespaces, allows tables, indexes, and temporary files to be created within the tablespace, and allows databases to be created that have the tablespace as their default tablespace.

Note that revoking this privilege will not alter the existence or location of existing objects.

### CONNECT 
Allows the grantee to connect to the database. This privilege is checked at connection startup (in addition to checking any restrictions imposed by pg_hba.conf).

### TEMPORARY 
Allows temporary tables to be created while using the database.

### EXECUTE 
Allows calling a function or procedure, including use of any operators that are implemented on top of the function. This is the only type of privilege that is applicable to functions and procedures.

### SET 
Allows a server configuration parameter to be set to a new value within the current session. (While this privilege can be granted on any parameter, it is meaningless except for parameters that would normally require superuser privilege to set.)

### ALTER SYSTEM 
Allows a server configuration parameter to be configured to a new value using the ALTER SYSTEM command.

### MAINTAIN 
Allows VACUUM, ANALYZE, CLUSTER, REFRESH MATERIALIZED VIEW, REINDEX, and LOCK TABLE on a relation.

PostgreSQL grants privileges on some types of objects to PUBLIC by default when the objects are created. No privileges are granted to PUBLIC by default on tables, table columns, sequences, foreign data wrappers, foreign servers, large objects, schemas, tablespaces, or configuration parameters.

For other types of objects, the default privileges granted to PUBLIC are as follows: CONNECT and TEMPORARY (create temporary tables) privileges for databases; EXECUTE privilege for functions and procedures; and USAGE privilege for languages and data types (including domains). The object owner can, of course, REVOKE both default and expressly granted privileges. (For maximum security, issue the REVOKE in the same transaction that creates the object; then there is no window in which another user can use the object.) Also, these default privilege settings can be overridden using the ALTER DEFAULT PRIVILEGES command.


## Privileges list
The privileges that have been granted for a particular object are displayed as a list of aclitem entries, each having the format:

```
grantee=privilege-abbreviation[*].../grantor
```

Each aclitem lists all the permissions of one grantee that have been granted by a particular grantor. Specific privileges are represented by one-letter abbreviations with * appended if the privilege was granted with grant option. For example, calvin=r*w/hobbes specifies that the role calvin has the privilege SELECT (r) with grant option (*) as well as the non-grantable privilege UPDATE (w), both granted by the role hobbes. If calvin also has some privileges on the same object granted by a different grantor, those would appear as a separate aclitem entry. An empty grantee field in an aclitem stands for PUBLIC.

As an example, suppose that user miriam creates table mytable and does:

```
GRANT SELECT ON mytable TO PUBLIC;
GRANT SELECT, UPDATE, INSERT ON mytable TO admin;
GRANT SELECT (col1), UPDATE (col1) ON mytable TO miriam_rw;
```

Then psql's \dp command would show privileges

If the “Access privileges” column is empty for a given object, it means the object has default privileges (that is, its privileges entry in the relevant system catalog is null). Default privileges always include all privileges for the owner, and can include some privileges for PUBLIC depending on the object type. The first GRANT or REVOKE on an object will instantiate the default privilegesand then modify them per the specified request. Similarly, entries are shown in “Column privileges” only for columns with nondefault privileges. (Note: for this purpose, “default privileges” always means the built-in default privileges for the object's type. An object whose privileges have been affected by an ALTER DEFAULT PRIVILEGES command will always be shown with an explicit privilege entry that includes the effects of the ALTER.)

Notice that the owner's implicit grant options are not marked in the access privileges display. A * will appear only when grant options have been explicitly granted to someone.

The “Access privileges” column shows (none) when the object's privileges entry is non-null but empty. This means that no privileges are granted at all, even to the object's owner — a rare situation. (The owner still has implicit grant options in this case, and so could re-grant her own privileges; but she has none at the moment.)