---
{"dg-publish":true,"permalink":"/developer/KeystoneJS/Keystone Calendar Day field set Default Value/","dgPassFrontmatter":true}
---


## Attempt 1
```ts
date: calendarDay({ defaultValue: new Date().toISOString() }),
```

> [!error] specifies defaultValue: 2023-04-26T21:22:45.395Z but values must be provided as a full-date ISO8601 string such as 1970-01-01


## Attempt 2
```ts
import { list } from "@keystone-6/core";
import { allowAll } from "@keystone-6/core/access";
import { calendarDay, relationship, text, } from "@keystone-6/core/fields";


const now = new Date();
const year = now.getFullYear();
const month = now.getMonth() + 1; // add 1 because getMonth() returns zero-based index
const day = now.getDate();
const today = `${year}-${month < 10 ? '0' + month : month}-${day < 10 ? '0' + day : day}`

export const Booking = list({

  access: allowAll,

  // todo hide these again
  // ui: {
  //   isHidden: true,
  // },


  fields: {
    date: calendarDay({ defaultValue: today }),
  },
})
```


> [!error] Error: Prisma schema validation - (get-dmmf wasm)
Error code: P1012
error: Error parsing attribute "@default": Parse error: "2023-04-26" is not a valid rfc3339 datetime string. (premature end of input)
  -->  schema.prisma:56
   | 
55 |   id         String    @id @default(uuid()) @postgresql.Uuid
56 |   date       DateTime? @default("2023-04-26") @postgresql.Date
   | 

## So Which is it?

the docs say to use ` ISO8601 format` but now the error is saying `is not a valid rfc3339`

[[developer/KeystoneJS/KeystoneJS\|KeystoneJS]]