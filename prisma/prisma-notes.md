## Prisma Summary

**Prisma** - Prisma is a modern ORM (Object-Relational Mapper) for Node.js and TypeScript.

-It helps developers build scalable and maintainable applications by providing an abstraction over the database.

### Installing Prisma

1. if you dont have a package json file you could start by running

```bash
npm init -y
```

- to install the package json file

2. install prisma

```bash
npm install prisma -D
```

or

```bash
npm install prisma --save-dev
```

- we are installing prisma in the development since we dont need it in the development

3. We run the command below:

```bash
npx prisma init --datasource-provider postgres
```

- where am saying postgres you can name any database you are using,,it can be something like,, sqlite, or mongodb etc

- so it generates a .env file,,also our schema.prisma file,,,
- the .env contains the connection to our db so you can include your username,,
  password ,,, and db you are using,,,like the db name!!! eg

```bash
DATABASE_URL="postgresql://username:randompassword@localhost:5432/mydb?schema=public"
```

**mydb**---here you say the name of the database you want to use,,

- if you go to \l on psql and see that the the db name doesnt dont panic because if you run migration the db name will be created!! i talk about migration later....
- #### NOTE: PRISMA uses postgres by defaul so if your db is postgres you can run the command without passing _--datasource-provider flag_

### MODELS

- rep different tables and their relationships,,

  **key concepts in prisma models**

1. data models
2. field types
3. relations
4. attributes

**fields are made up of 4 differenty types**

1. field name(required)
2. data types eg strings
3. field type modifier(optional)
4. attributes(optional)

**field attributes include things like**

1. **@id** marks the field as the primary key of the model.

2. **@default** : sets a default value for the field eg createdAt DateTime @default(now())

3. **@unique**- ensures that a field has unique values across all rows in a table.

4. **@updateAt**-automatically updates a field to the current timestamp whenever the record is updated eg updatedAt DateTime @updatedAt.

5. **@map** - maps the field to a column with a different name in the database for example firstName String @map("first_name")

6. **@relation** -defines relationships between models. Specifies foreign keys and the related models eg author User @relation(fields: [authorId], references: [id])

7. **@@map**- it is a model attribute, and it maps the model to a table with a different name in the database.

### CREATING MODELS

- Models are defined inside schema.prisma
  EXAMPLE:
- lets create model User

```js
model User{

id String @id @default(uuid())
firstName String
lastName String
username String @unique
emailAddress String @unique
isDeleted Boolean @default(false)
@@map("users")

}
```

### Migration!!!!

- anytime you make changes inside the scehema file you should run migration inorder the changes to be migrated

- command :

```bash
npx prisma migrate dev --name "created users table"
```

- **NOTE**: you have to name youre migrate,, just like we name commit messages

- After this a Prisma Client is generated,,, this is what we take to production this helps us communicaate with the database

- now if you run in /l in psql you will see the db name if it wasnt created at first also \d to see the table,, user..

## PRISMA CLIENT

- PRISMA client is auto generated and type safe db client that you use to interact with your database..

- to generate a client we use this command thou nowadays in is automatically generated,, if its not availabe run
- npx prisma generate and next
  run -- npm install @prima/client

- so this gives us a set of Apis to create and perform crud operations

### CRUD OPERATIONS

1. TO CREATE USE THE create eg

```js
const newProduct = await client.user.create({
  data: {
    //the user object to post on the table user
  },
});
```

- client is imported from prisma client

```js
import { PrismaClient } from "@prisma/client";
const client = new PrismaClient();
```

2. **CreateMany**

- this method is used to create many things at once ,,
- it takes in an array
- like this:

```js
const newProduct = await client.user.createMany({
  data: [
    {
      //user one object
    },
    {
      // user two object
    },
  ],
});
```

## CRUD OPERATIONS (READ)

- Get all records using **findMany()**
- Find the first record that matches a criteria using **findFirst()** and apply the filter using **where**
- Specify the fields you want returned with the **select** field
- Applying the **AND** operator to select only records that match multiple criterias
- Applying **OR** operator to select records that match any of the Criteria

## comparison operators in Prisma

### Numeric filters

**equals**: matches exactly _eg { age: { equals: 25 } }_

**not**: matches 'not equal to the specified value' _eg { age: { not: 25 } }_

**lt**: less than _eg { age: { lt: 18 } }._

**lte**: less than or equal to _eg { age: { lte: 18 } }._

**gt**: greater than _eg { age: { gt: 18 } }._

**gte**: greater than or equal to _eg { age: { gte: 18 } }._

### String filters

**equals**: matches exactly (case-sensitive) eg { name: { equals: "John" } }

**contains**: matches if the string contains the specified substring eg { name: { contains: "John" } }

**startsWith**: matches if the string starts with the specified string eg { name: { startsWith: "Jo" } }.

**endsWith**: matches if the string ends with the specified string eg { name: { endsWith: "hn" } }.

**not**: negates the condition eg { name: { not: { contains: "Doe" } } }.

### Update

1. Update a single record using **update()**
2. Update multiple records using **updateMany()**

### Delete

1. Delete a single record using delete()
2. Delete multiple records using deleteMany()

## Relationships

- Relationships define how different models (tables) in a database are connected to each other.

- Prisma uses a schema to define these relationships making it easier to work with relational databases.

**The types of relationships include:**

**one-to-one relationship (1-1)**: means that one record in a table is associated to only one record in another table.

**one-to-many relationship (1-n)**: means that one record in a table can be associated with multiple other records in another table.

**many-to-many relationship (m-n)**: means that multiple records in one table can be associated to multiple records in another table.
