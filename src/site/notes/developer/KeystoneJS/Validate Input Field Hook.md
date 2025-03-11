---
{"dg-publish":true,"permalink":"/developer/KeystoneJS/Validate Input Field Hook/","dgPassFrontmatter":true}
---

comparison of how you could sanitize data, or just throw a `validation` error upon POST

```ts
slug: text({
      validation: { isRequired: true },
      isIndexed: 'unique',
      hooks: {
        beforeOperation({resolvedData}) {
          if(!resolvedData?.slug) return console.log('Category: no slug')
          resolvedData.slug = slugFormat(String(resolvedData.slug))
        },
        validateInput: ({ addValidationError, resolvedData, fieldKey }) => {
          const inputValue = resolvedData[fieldKey];

          if (!inputValue) return
          // @ts-ignore //todo might cause problems
          if (!inputValue.match(/^[a-z0-9]+(?:-[A-Za-z0-9]+)*$/)) {
            addValidationError(`Can only contain lower case "a-z" and dash "-" characters.`);
          }
        },
      }
    }),
```

[[developer/KeystoneJS/KeystoneJS\|KeystoneJS]]