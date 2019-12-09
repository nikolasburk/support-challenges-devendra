Hey @username, `preview017` version has some breaking changes.

Since `preview017` version, the Photon.js code is now generated into `node_modules/@prisma/photon` instead of `node_modules/@generated/photon` 

Please import the Photon from `@prisma/photon`  in your `src/index.ts` like below:

```typescript
import { Photon } from '@prisma/photon'
```

This should resolve your issue :)
