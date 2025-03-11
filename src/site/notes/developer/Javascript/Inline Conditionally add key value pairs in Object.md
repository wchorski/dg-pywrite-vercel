---
{"dg-publish":true,"tags":["javascript","webdev","JSON","OOP","ternary"],"permalink":"/developer/javascript/inline-conditionally-add-key-value-pairs-in-object/","dgPassFrontmatter":true}
---

conditionally add in a key value pair or nothing at all. Avoid `null` or `undefined` key values being added to the object.

```js

const condition = true

const myObject = {
  ...(condition ? { key: value } : {})
};

```


---
## Credits
- [[developer/developer_box📦\|developer_box📦]]
- [[developer/Javascript/Javascript\|Javascript]]