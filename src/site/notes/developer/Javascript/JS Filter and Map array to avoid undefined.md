---
{"dg-publish":true,"permalink":"/developer/javascript/js-filter-and-map-array-to-avoid-undefined/","dgPassFrontmatter":true}
---

when you map over an array with an `if` statement, you'll notice it fills your array with `undefined` values.

a quick and clean way to fix this is to use a `filter` before `map`

```js
const selectedAddons = addons
  .filter((add) => values.addonIds.includes(add.id))
  .map((add) => add.name);

console.log(selectedAddons);

```

---
## Backlinks
- [[developer/Javascript/Javascript\|Javascript]]