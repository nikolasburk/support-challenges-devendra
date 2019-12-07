# Challenge-4

### Process to figure out a solution

#### Key points to note:
- This particular problem has to do with Graphql rather than Prisma.
- User is looking for suggestion or steps in order to fulfil his desired features.
- So user is expecting an approach rather than a solution, still I have provided both.
- Database related part of the problem is about 
	- adding a field to `schema.prisma` file and saving it while `prisma2 dev` mode is running which will take care of the migration and photon generation
	- adding the `views` in model definition of `Post` in nexus at application level
- Graphql API related part is about exposing two fields
	- `users` : which will give list of users which doenst accept any arguments of its own
	- `topPosts` : which will be part of the `User` model ( at application level, in nexus ) and can accept optional argument for number of top posts

### Solution:

Adding `views` to `Post`:  

1. Add `views` field in `prisma/schema.prisma` as below and save the file

```typescript
....

model Post {
  id        String   @default(cuid()) @id
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt
  published Boolean  @default(true)
  title     String
  content   String?
  author    User?
  
  views     Int @default(0)
}
...
```

Prisma Framework's developement mode will perform required migrations and photon generation

2. Add `views` to `Post` definiton at application level in nexus as follows in `src/schema.ts`

```typescript
...

const Post = objectType({
  name: 'Post',
  definition(t) {
    t.model.id()
    t.model.createdAt()
    t.model.updatedAt()
    t.model.title()
    t.model.content()
    t.model.published()
    t.model.author()
    
    t.model.views()
  },
})

...
```
 

Adding ability to query top posts of user 

1. Expose  `users` field/query under main `Query` with follwoing modification in `src/schema.ts`

```typescript
const Query = objectType({
  name: 'Query',
  ...
  ...
  
  t.list.field('users', {
    type: 'User',
    resolve: (_parent, _args, ctx) => {
      return ctx.photon.users.findMany({
        include: {
          posts: true
        }
      })
    }
  })
  
 },
})
```

2. Add `topPosts` field which accepts optional argument for number of top posts, to `User` model in nexus as below

```typescript
const photon = new Photon()
const User = objectType({
  name: 'User',
  definition(t) {
    t.model.id()
    t.model.email()
    ...

    t.list.field('topPosts', {
      type: 'Post',
      args: {
        number: intArg({ nullable: true }),
      },
      resolve: (parent, { number }, ctx) => {
        const userId = parent.id
        
        let input: any = {
          where: {
            author: {
              id: userId
            }
          },
        }
        if(number > 0) {
          input.first = number
        }
        return ctx.photon.posts.findMany(input)
      },
    })

  },
})
```

### Reply to user (in markdown):

Hey @username,...
