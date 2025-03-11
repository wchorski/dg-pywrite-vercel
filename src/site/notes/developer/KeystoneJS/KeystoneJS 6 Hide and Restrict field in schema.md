---
{"dg-publish":true,"tags":["KeystoneJS","typescript","authorization"],"permalink":"/developer/keystone-js/keystone-js-6-hide-and-restrict-field-in-schema/","dgPassFrontmatter":true}
---

I have a data schema named **Booking** in my [[developer/KeystoneJS/KeystoneJS\|KeystoneJS]] app. The [Docs](https://keystonejs.com/docs/config/access-control)explain in a broad term, but examples for an individual field access is still confusing. Here is my example with permissions, access, etc.
## Access
- **Customer**: Can only view their certain booking `canViewBookings`.
- **Employees** Can update booking assigned to them.

How I set the access for the whole **Booking** schema
```ts
access: {
    filter: {
      query: rules.canViewBookings,      
      update: rules.canManageBookings,
      delete: () => false,
    },
    operation: {
      create: permissions.canManageBookings,
      query: permissions.isLoggedIn,
      update: permissions.isLoggedIn,
      delete: () => false,
    }
  },
```

## No Permissions Set
Currently both **Customer** and **Employee** can view all fields on the **Booking** schema.

![attachments/Pasted image 20240718183126.png](/img/user/attachments/Pasted%20image%2020240718183126.png)
## Secret Notes Field
I created a `secretNotes` field for recording booking/client specific notes that we don't want the **Customer** to see. Here we use

1. `access: { read: ...`: to not allow the data to be queried 
2. `fieldMode`: to hide the field in the [[developer/KeystoneJS/KeystoneJS\|KeystoneJS]] UI

```ts
secretNotes: text({
      ui: {
        description: 'notes only visible between management and employees. NOT the customer',
        displayMode: 'textarea',
        
        itemView: {
          fieldMode: ({ session, context, item }) => permissions.canManageBookings({session}) ? 'edit': 'hidden',
        },
      },
      access: {
        read: ({ session, context, listKey, fieldKey, operation, item }) => permissions.canManageBookings({session})
      }
    }),
```
## Almost but not quite
Just having the `access: { read: ...` hides the data inside the `secreNotes` field (and prevents frontend from accidental query) but still leaves the label to that input letting the **Customer** know that we keep secrets 🤫

![attachments/Pasted image 20240718182819.png](/img/user/attachments/Pasted%20image%2020240718182819.png)
## Completely Hidden
Adding `fieldMode: ({ session, context, item }) => permissions.canManageBookings({session}) ? 'edit': 'hidden',` dynamically tells the Keystone UI to either show or hide the entire field depend on *permissions*

![attachments/Pasted image 20240718183248.png](/img/user/attachments/Pasted%20image%2020240718183248.png)