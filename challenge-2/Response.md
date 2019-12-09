Hey @username,

**TL;DR:**

**Given your requirement and the ease of interaction with MySQL db that you would expect while not dealing with too low level stuff, Photon.js is the best fit for you.**


Also, Photon.js is an auto-generated database client that enables type-safe database access and reduces boilerplate.

You can use it as an alternative to traditional ORMs such as Sequelize or  Knex.js.

Photon.js is part of Prisma2 (Prisma Framework) which is a database workflow framework that will:
    - take care of talking to your database in type safe manner (Photon) and 
    - will cater to your database migrations needs (Lift)

These are a couple of highlights of Prisma2.

Since (Prisma2) Prisma Framework is modular it gives you freedom to choose what you need as per your requirements.

You can use Photon for data access and use Lift for migrations, which we would encourage you to use since it will be good for you to keep track of your schema changes in systematic way.

You can give it a try with examples [here](https://github.com/prisma/prisma-examples)
