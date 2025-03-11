---
{"dg-publish":true,"permalink":"/developer/KeystoneJS/KeystoneJS Prisma Migration/","dgPassFrontmatter":true}
---

migrating an existing [[developer/Finance/PostGreSQL\|PostGreSQL]] database with the [[developer/KeystoneJS/KeystoneJS\|KeystoneJS]] ORM takes a bit of doc reading. Essentially mashing the [Command Line - Keystone 6 Docs](https://keystonejs.com/docs/guides/cli) and [link](https://www.prisma.io/docs/guides/migrate/developing-with-prisma-migrate/add-prisma-migrate-to-a-project#introspect-to-create-or-update-your-prisma-schema) articles together

while in the `./backend` keystonejs root directory

```bash
mkdir -p migrations/0_init
```

```bash
npx prisma migrate diff \
--from-empty \
--to-schema-datamodel schema.prisma \
--script > migrations/0_init/migration.sql
```

```bash
npx prisma migrate resolve --applied 0_init
```

run below command to double check

```bash
yarn keystone build && keystone prisma migrate deploy
```

## File tree for reference

```
.
├── Dockerfile
├── README.md
├── access.ts
├── auth.ts
├── crontab
│   └── subscription_cron.ts
├── keystone.ts
├── migrations
│   └── 0_initial
				migration.sql <---- ** here
├── mutations
│   ├── addToCart.ts
│   ├── checkout.ts
│   ├── contact.ts
│   └── sampleMutation.ts
├── package.json
├── prepareToUpload.ts
├── public
│   ├── assets
│   ├── seedfiles
│   └── styles
├── schema.graphql
├── schema.prisma
├── schema.ts
├── schemas
├── tsconfig.json
├── types.ts
└── yarn.lock
```


## Still not working

> [!warning] problem
> Still having problems with this build. For now I connect my **dev app environment** with my **production database** and run `yarn dev` to update the schema, probably not a good idea but it's what works for now


