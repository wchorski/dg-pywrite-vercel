---
{"dg-publish":true,"tags":["css","style","html","webdev"],"permalink":"/developer/html/heading-with-subtitle-above-or-below/","dgPassFrontmatter":true}
---

Setting the subtitle heading is a confusing game of symatics and personal style. There must be a definitive way. Enter `role="doc-subtitle"`

```html
<h1>
	<span role="doc-subtitle">subscription</span>
	CuteFruitAdmin
</h1>
```

## As Above so Below
With some newer CSS magic (supported in all browsers) we can scale the text in realtion to the size of the parent
```css
[role="doc-subtitle"] {
	display: block;
	color: grey;
	font-size: 4cqh;
}
```

## Example Screenshot 
![attachments/Pasted image 20250228181325.png](/img/user/attachments/Pasted%20image%2020250228181325.png)

---
## Credit
- https://css-tricks.com/html-for-subheadings-and-headings/
- https://stackoverflow.com/questions/30693928/how-to-make-font-size-relative-to-parent-div