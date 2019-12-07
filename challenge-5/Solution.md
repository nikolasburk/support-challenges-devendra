# Challenge-5

### Process to figure out solution

#### Key points to note:
- User upgraded the Prisma2 version to preivew017 which is like to have some breaking changes since its in preview mode
- User has metioned that he is facing issue while runing his code rather than facing any issues in Prisma framework's workflow like schema migration or photon gneration.
- Considering Photon code generation and schema migrations are running ok we will look more into the code
- In cases where there are version upgrades are involved we must refer to release notes or change logs.
- Release notes of `preview017` clearly states it has breaking changes and upon further explicitly states the change with respect to Photon.js
- As user was facing issue with the exact breaking change mentioned in release notes.
- The line `1 import { Photon } from '@generated/photon'` from error log clearly suggest the issue and the solution as well.


### Solution:
 

### Reply to user (in markdown):

Hey @username, preview017 version has some breaking changes.

Since preview017 version the Photon.js code is now generated into `node_modules/@prisma/photon` instead of `node_modules/@generated/photon` 

Please import the Photon from `@prisma/photon`  in your `src/index.ts` like below:

```typescript
import { Photon } from '@prisma/photon'
```

This should resolve your issue :)
