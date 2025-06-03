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

# Priviledges
