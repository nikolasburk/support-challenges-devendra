Hey @username, Adding `views` on posts to keep score to indicate how often that post has been viewed is pretty straight forward in your setup. How and when to update the views count is something you cant take a call on depending on requirement.

While ability to query the top `n` posts of a user can be done by adding field and its resolver on `User` field at application level rather than database level. As for now we assume the top posts depend on the `views` count on Post field.

I have added code snippets below for your reference along with description.

If you need any further help, please feel free to reach out :)


#### Adding `views` to `Post`:  

Note: Depending on the requirement the logic of incrementing the `views` count on `Post` would change, hence avoiding that particular implementation. However simple thing would be to increment it either when we query a single post(by unique identifier) or when we query a post(by unique identifier) along with its content.

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
  // Adds views on Post model
  views     Int @default(0)
}
...
```

Prisma Framework's development mode will perform required migrations and photon generation.

2. Add `views` to `Post` definition at application level as follows in `src/schema.ts`

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
    // Adds views on Post model definition
    t.model.views()
  },
})

...
```
 

#### Adding ability to query top posts of user 

1. Expose  `users` field/query under main `Query` with following modification in `src/schema.ts`

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
          orderBy: {
            views: "desc"
          }
        }
        if(number && number > 0) {
          input.first = number
        }
        return ctx.photon.posts.findMany(input)
      },
    })

  },
})
```