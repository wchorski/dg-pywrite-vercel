---
{"dg-publish":true,"tags":["webdev","frontend","style"],"permalink":"/developer/CSS/Modern Use of Media Queries/","dgPassFrontmatter":true}
---

[[developer/CSS/CSS\|CSS]] Magic for easy to use dynamic columns.

```css
.wrapper {
	container-type: inline-size;
}

.columns {
	display: grid;
	gap: 2rem;
}

// 3 is the number *columns*
@container(width > calc(20ch * 3)) {
	.columns {
		grid-template-columns: repeate(3, 1fr);
	}
}
```

```html
<div class="wrapper">
	<div class="columns">
		<p> ... </p>
		<p> ... </p>
		<p> ... </p>
		<p> ... </p>
		<p> ... </p>
		<p> ... </p>
	</div>
</div>	
```
## Credit
- [This changed how I use media queries (youtube.com)](https://www.youtube.com/watch?v=e6WuFNRP7e8)