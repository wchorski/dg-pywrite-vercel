---
{"dg-publish":true,"permalink":"/developer/html/html-dialog-model-for-accessable-tooltips-pop-ups-and-more/","dgPassFrontmatter":true}
---


## Open and close 
```js
const dialog = document.querySelector("dialog")

dialog.show() // Opens a non-modal dialog
dialog.showModal() // Opens a modal

dialog.close() // Closes the dialog
```

## Style tooltip
```css
dialog {
  z-index: 10;
  margin-top: 10px;
  background: green;
  border: none;
  border-radius: 1rem;
}
```

## Style Backdrop
```css
dialog::backdrop {
  background-color: hsl(250, 100%, 50%, 0.25);
}
```

## Pop up Form
```html
<dialog>
  <form>
    <input type="text" />
    <button formmethod="dialog" type="submit">Cancel</button>
    <button type="submit">Submit</button>
  </form>
</dialog>
```

## Close Box if user clicks outside of Element
```js 
const dialog = document.querySelector("dialog") 

dialog.addEventListener("click", e => {
  const dialogDimensions = dialog.getBoundingClientRect()
  if (
    e.clientX < dialogDimensions.left ||
    e.clientX > dialogDimensions.right ||
    e.clientY < dialogDimensions.top ||
    e.clientY > dialogDimensions.bottom
  ) {
    dialog.close()
  }
})
```

## Credits
- [Modals Will Never Be The Same - HTML dialog Element (webdevsimplified.com)](https://blog.webdevsimplified.com/2023-04/html-dialog/)
- [The New dialog HTML Element Changes Modals Forever - YouTube](https://www.youtube.com/watch?v=ywtkJkxJsdg)

## Backlinks
- [[developer/HTML/HTML, Back to Basics\|HTML, Back to Basics]]