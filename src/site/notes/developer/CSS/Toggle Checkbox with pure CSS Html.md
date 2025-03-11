---
{"dg-publish":true,"tags":["css","style","html","scss","no_js"],"permalink":"/developer/css/toggle-checkbox-with-pure-css-html/","dgPassFrontmatter":true}
---

## Live Demo
<iframe src="https://assets.tawtaw.site/tawtaw/checkbox/index.html" width="100%" height="50"></iframe>

## Html

```html
<label>
	<input type="checkbox" />
	<span> Check Me Out! </span>
</label>
```

> [!tip] the `<span>` tag isn't needed but is nice to have once you start applying any `grid` or `flex`
## CSS
```css
label:has(input[type="checkbox"]) {
	cursor: pointer;
}

input[type="checkbox"] {
	appearance: none;
	position: relative;
	display: inline-block;
	background: lightgrey;
	height: 1.65rem;
	width: 2.75rem;
	vertical-align: middle;
	border-radius: 2rem;
	box-shadow: 0px 1px 3px #0003 inset;
	transition: 250ms linear background;
	cursor: pointer;
}

input[type="checkbox"]::before {
	content: "";
	display: block;
	width: 1.25rem;
	height: 1.25rem;
	background: #fff;
	border-radius: 1.2rem;
	position: absolute;
	top: 0.2rem;
	left: 0.2rem;
	box-shadow: 0px 1px 3px #0003 inset;
	transition: 0.25s linear transform;
	transform: translateX(0rem);
}
input[type="checkbox"]:checked {
	background: limegreen;
}
input[type="checkbox"]:checked::before {
	transform: translateX(1rem);
}

/* accessablility */
input[type="checkbox"]:focus-within {
	outline: 2px solid dodgerblue;
	outline-offset: 2px;
}
input[type="checkbox"]:focus {
	outline-color: transparent;
}

```
## Previous Method

### SCSS
```scss
input[type="checkbox"].toggle {
  opacity: .1;
  position: absolute;
  cursor: pointer;
  left: -9000px;
  top: -9000px;

  &:disabled + label {
    color: #68346873;

    &::before{
      color: #68346873;
      background-color: #68346873;
    }
    &::after{
      color: #68346873;
      background-color: #68346873;
    }
  }
}


input[type="checkbox"].toggle + label {
  display: flex;
  align-items: center;
  position: relative;
  cursor: pointer;

  &::before{
    content: "";
    width: 4.5em;
    height: 1.2rem;
    background-color: #ffffff2b;
    border-radius: 1em;
    margin-right: .25rem;
  }

  &::after{
    content: "\2715";
    width: 2em;
    height: 2em;
    border-radius: 1em;
    background-color: grey;

    display: flex;
    justify-content: center;
    align-items: center;
    position: absolute;
    transition: 200ms ease-in-out;
  }
}

input[type="checkbox"].toggle:checked + label {

  color: var(--color-highlight);

  &::after{
    content: "\2713";
    color: black;
    transform: translateX(115%);
    background-color: var(--color-highlight);
  }
}
```

---

## Backlinks
- [Don't Use JS for That: Moving Features to CSS and HTML by Kilian Valkhof (youtube.com)](https://www.youtube.com/watch?v=IP_rtWEMR0o)
- [[developer/CSS/Cascading Style Sheet Tips & Tricks\|Cascading Style Sheet Tips & Tricks]]