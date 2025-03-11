---
{"dg-publish":true,"tags":["javascript","input"],"permalink":"/developer/Javascript/Auto Resize Height of Textarea on Change/","dgPassFrontmatter":true}
---

Quick fix to auto resize a `<textarea />` if the input starts to go outside the pre-determined height with [[developer/Javascript/Javascript\|Javascript]]

```jsx
<textarea onChange={(e) => {
  console.log('scroll h, ', e.target.scrollHeight);
  e.target.style.height = e.target.scrollHeight + 'px'
}}/>
```

```css
textarea{
	overflow: hidden;
	resize: vertical;
}
```