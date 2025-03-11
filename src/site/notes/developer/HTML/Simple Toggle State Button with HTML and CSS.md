---
{"dg-publish":true,"permalink":"/developer/HTML/Simple Toggle State Button with HTML and CSS/","dgPassFrontmatter":true}
---

I found a little trick using an `input checkbox` as a sort of page specific state machine. I know the title says button, but figured you're here because you had 'button' in your search query

anyway...

```html
<label htmlFor="nav_checkbox" id='navdrawer-cont' className='toggle-menu-btn'>
	
	<input type='checkbox' id='nav_checkbox' />

	
	<i />
</label>
```
```scss
#checkbox-container{

	input[type=checkbox]{
		display: hidden;
	}
	
	& :has(input[type=checkbox]:checked){
		svg{
			background-color: var(--c-1);
			color: var(--c-txt-rev);
		}
	}
}
```

You'll see I wrap an icon and the input in a `<label />` tag so it acts as one unit, then I hide the checkbox

--- 
## Credits
- [How To Create a Responsive Navigation Menu Bar using HTML and CSS | Mobile First - YouTube](https://www.youtube.com/watch?v=OotCLwM0-bY)

[[developer/developer_box📦\|developer_box📦]]