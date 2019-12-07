# Challenge-3


* Solution fot this challenge can be found at [Challenge-3](https://github.com/devendradhanal/support-challenge-3)

### Process to figure out a solution

Key points to note:
- User is facing problem in production so data is critical.
- Find out how Lift can assist with this particular situation
- Prisma Framework( Prisma2 ) Lift in particular supports migrations along with hook feature to extend the migration workflow and allow custom scripts
- Since Lift's migration hook feature is yet to be implemeted ( as described here: https://github.com/prisma/lift/issues/195 ) we cant suggest this solution for now.
- We should provide a workaround if possible for given situation.

- I followed the steps described below to solve the issue.

### Solution: 

### Reply to user (in markdown):

Hey @username, Prisma2's (Prisma Framework ) Lift supports extending the migration workflow with before and after hooks with custom script where
we can handle such scenarions.

The faeature however is still in developement and you can expect it very soon.

Meanwhile We would like to suggest a workaround.


Steps:
1. Run `npx prisma2 dev`
2. Add `firstName` and `lastName` as nullable fields in the `prisma/schema.prisma`

2.1. First add `firstName` as below :

```
generator photon {
  provider = "photonjs"
}

datasource db {
  provider = "sqlite"
  url      = "file:dev.db"
}

model User {
  id    String  @default(cuid()) @id
  name  String
  firstName  String?
}
``` 
and save the file.
Prisma will run the migration and will add the firstName field for all users in the db

2.2. Similarly repeat for `lastName` field as below
	
```
generator photon {
  provider = "photonjs"
}

datasource db {
  provider = "sqlite"
  url      = "file:dev.db"
}

model User {
  id    String  @default(cuid()) @id
  name  String
  firstName  String?
  lastName  String?
}

```
and save the file. 
After Prisma runs the migration your db will have `firstName` and `lastName` fields for all users in the db


3. Run custom script to split the name into first and last name columns
 ```typescript
 import { Photon } from '@generated/photon'

const photon = new Photon()

// A `main` function so that we can use async/await
async function main() {
  // Read the database with users with id and name, firstName and lastName
  const allUsers = await photon.users.findMany({
    select: {
      id: true,
      name: true,
      firstName: true,
      lastName: true
    }
  })
  console.log('All users Before migration step 1')
  console.log(allUsers)
  for (const currentUser of allUsers) {
    const [firstName, lastName] = currentUser.name.split(' ')
    await photon.users.update({
      where: {
        id: currentUser.id
      },
      data: {
        firstName,
        lastName
      }
    })
  }
  const _allUsers = await photon.users.findMany({
    select: {
      id: true,
      name: true,
      firstName: true,
      lastName: true
    }
  })
  console.log('All users After migration step 1')
  console.log(_allUsers)
}

main()
  .catch(e => console.error(e))
  .finally(async () => {
    await photon.disconnect()
  })

 ``` 
 
4. Now you can either keep the name field if you want Or
	you can  delete it from `prisma/schema.prisma` while running in dev mode where Prisma will ask whether you would like to remove the name field which will delete the `name` columd from db.

5. Now as per your schema requirement you can either keep the first and last name fields as optional or simply make them required by modifying `prisma/schema.prisma` and save the file.


And yes you still can use Prisma after this :)

Hope this helps and please feel free to reach out in case you need any further asistance.

