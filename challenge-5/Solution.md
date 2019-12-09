# Challenge-5


* Solution for this challenge can be found at [Challenge-5](https://github.com/devendradhanal/support-challenge-5)

### Process to figure out solution

#### Key points to note:
- User upgraded the Prisma2 version to `preview017` which is like to have some breaking changes since it's in preview mode
- The user has mentioned that he is facing issues while running his code rather than facing any issues in the Prisma Framework's workflow like schema migration or photon generation.
- Considering Photon code generation and schema migrations are running ok we will look more into the code
- In cases where there are version upgrades that are involved, we must refer to release notes or change logs.
- Release notes of `preview017` clearly say it has some breaking changes and upon further explicitly states the change concerning Photon.js
- As the user was facing the issue with the exact breaking change mentioned in release notes.
- The line `1 import { Photon } from '@generated/photon'` from error log clearly suggests the issue and the solution as well.


### Solution:
 

### Reply to user (in markdown):

Hey @username, `preview017` version has some breaking changes.

Since `preview017` version, the Photon.js code is now generated into `node_modules/@prisma/photon` instead of `node_modules/@generated/photon` 

Please import the Photon from `@prisma/photon`  in your `src/index.ts` like below:

```typescript
import { Photon } from '@prisma/photon'
```

This should resolve your issue :)
