# Challenge-1

### Process to figure out a solution

##### Key points to note:
- User upgraded the Prisma2 version to preivew016 which is like to have some breaking changes since it's in preview mode
- The `prisma2 dev` command internally runs `prisma2 generate` and `prisma2 lift save`, `prisma2 lift up` but among these commands `prisma2 generate` is also fired as part of `postinstall` and there the project didn't run into an error. So basically Photon is not the issue here. 
- And more clearly it's stated in the error log that it has something to do with migration steps.
- In such cases, we must refer to release notes or changelogs
- Found the exact error description as of this project issue in release notes of preview-016 and the solution as well.

#### Solution:
- Delete migrations folder from the repo manually

    `rm -r prisma/migrations`

- Delete the `_Migration` table from schema manually by connecting to your database

    `drop table "_Migration";`

- Run `prisma2 dev` command to run the photon generation and lift migration


#### Reply to the user (in markdown):

Hey @username, the issue you are facing is due to a major refactor in the Lift migration engine introduced in `preview-016`.

Please delete your folder at `prisma/migrations` and also delete `_Migration` table in your schema in the PostgreSQL database. Now run `prisma2 dev`.

This should resolve your issue :)

Also since we are in the preview phase we expect such breaking changes.
