---
title: "Migrations and seeds in Sequelize"
excerpt: "Simple & easy example of migration and seed in Sequelize ORM"
last_modified_at: 2017-03-09T10:27:01-05:00
categories:
  - ORM
tags: 
  - Sequelize
  - JavaScript
---

<!-- short intrduction -->
## Migrations

Migrations in sequelize are simple way to create new table in database. It works in terminal, but first have to install cli (required only for the first time):

```js
npm install --save-dev sequelize-cli
```

Now You can create new model (representation of table in database). 

Note: small letters are less problematic to use in column names in SQL databases so I prefer "underscore" naming convention f.e. "company_id" instead of "companyId"

```js
npx sequelize-cli model:generate --name user
 --attributes first_name:string,
 last_name:string,email:string,company_id:integer
```
The result will be file "users.js" in "models" directory.

Next step is to migrate new model to database:

```js
npx sequelize-cli db:migrate
```

This will create table "users" in database. 

Note: Sequelize during table creation will change model name to plural form, so for model "user" table will have name "users".

To undo last migration execute:
```js
npx sequelize-cli db:migrate:undo
```

## Seeds

Now You have empty table, so let's insert some data. Files with initial data are called "seeds". 

```js
npx sequelize-cli seed:generate --name first-users
```
After this command You will have new seed file in folder  "seeders" with name "XXXXXXXXXXXXXX-first-users.js" where XX... are unique id based on creation time.

Next step is to edit this file:
```js
module.exports = {
  up: (queryInterface, Sequelize) => {
    return queryInterface.bulkInsert('users', [{
      first_name: 'Max',
      last_name: 'Json',
      email: 'max@json.com',
      company_id: 1,
      created_at: new Date(),
      updated_at: new Date()
    },
    {
      first_name: 'Eva',
      last_name: 'Json',
      email: 'eva@json.com',
      company_id: 1,
      created_at: new Date(),
      updated_at: new Date()
    }]);
  },
  down: (queryInterface, Sequelize) => {
    return queryInterface.bulkDelete('users', null, {});
  }
};
```
It will create two records in database (users Max and Eva).

Note: seeds executions in Sequelize are not remembered in database, so each time all seeds are proceed (You will add the same records each time), unless You specify one seed to run.

Bellow command run all seeds - best to use for the first time for multiple seeds:
```js
npx sequelize-cli db:seed:all
```
Undo all seeds:
```js
npx sequelize-cli db:seed:undo:all
```

If You want to add only one specific seed:
```js
npx sequelize-cli db:seed --seed XXXXXXXXXXXXXX-first-users
```
Undo last seed:
```js
npx sequelize-cli db:seed:undo
```

That's all!
