---
{"dg-publish":true,"permalink":"/developer/javascript/javascript-math-functions/","dgPassFrontmatter":true}
---

## Don't go past Zero

helpful for math about pricing where the number should not go below zero, as a negative number

```js
let num = 5

num = Math.max(num - 9, 0)

console.log(num) //0
```