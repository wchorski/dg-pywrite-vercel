---
{"dg-publish":true,"tags":["javascript"],"permalink":"/developer/javascript/how-to-remove-duplicates-in-array/","dgPassFrontmatter":true}
---


Filter out any duplicates and return a single array of unique categories in [[developer/Javascript/Javascript\|Javascript]]

```javascript
const array = ['apple', 'apple', 'orange', 'pear', 'banana']
const uniq = [...new Set(array)];
```

*( Note that var* `uniq` will be an array... `new Set()` turns it into a set, but `[...]` turns it back into an array again )

---

## Credits
- [javascript - Remove duplicate values from JS array - Stack Overflow](https://stackoverflow.com/questions/9229645/remove-duplicate-values-from-js-array)