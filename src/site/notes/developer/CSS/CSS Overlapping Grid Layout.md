---
{"dg-publish":true,"tags":["style","css","scss","html","layout","webdev"],"permalink":"/developer/CSS/CSS Overlapping Grid Layout/","dgPassFrontmatter":true}
---

[React - JSFiddle - Code Playground](https://jsfiddle.net/r2x6z3Lt/51/)

How to overlap elements such as a stylish sidebar that hovers over other elements
## Html
```html
<main className="layout-grid">
	<header> header </header>
	<div className="content">
	  <p>content</p>
	  <p>content</p>
	  <p>content</p>
	  <p>content</p>
	</div>
	<aside>overlap sidebar</aside>
	<footer>footer</footer>
</main>
```

## Sass
```scss
.layout-grid {
  background-color: palegoldenrod;
  display: grid;
  grid-template-columns: repeat(5, 1fr);
  min-height: 10vh;
  min-width: 100vw;
  border: dotted red 1px;
  

  > header {
    background: #18A4E0;
    grid-column: 1/4;
  }

  > .content {
    min-height: 10rem;
    border: dashed grey 1px;
    grid-row: 2;
    grid-column: 3;
  }
  
  > aside {
    background: #ff7b006b;
    grid-column: 4;
    grid-row: 1/4;
  }
  
  > footer {
    background: #22ff006b;
    grid-column: 1/-1;
    grid-row: 3;
  }
}
```