---
{"dg-publish":true,"permalink":"/developer/javascript/js-non-null-and-optional-chaining/","dgPassFrontmatter":true}
---



describe_the_problem

```tsx
// 👇️ optional chaining (?.)
    inputRef.current?.focus();
```


```tsx
// 👇️ using non-null (!) assertion
    inputRef.current!.focus();
```

---
## Credits
- [link](https://bobbyhadz.com/blog/react-useref-object-is-possibly-null#:~:text=The "Object is possibly null" error is caused,whose.current property is initialized to the passed argument.)

## Backlinks
- [[developer/Javascript/Javascript\|Javascript]]