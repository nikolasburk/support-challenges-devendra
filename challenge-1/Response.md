Hey @username, the issue you are facing is due to a major refactor in the Lift migration engine introduced in `preview-016`.

Please delete your folder at `prisma/migrations` and also delete `_Migration` table in your schema in the PostgreSQL database. Now run `prisma2 dev`.

This should resolve your issue :)

Also since we are in the preview phase we expect such breaking changes.
