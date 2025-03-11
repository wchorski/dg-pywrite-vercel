---
{"dg-publish":true,"permalink":"/developer/ReactJS/Remove, Clear a File Input/","dgPassFrontmatter":true}
---

There is an extra step that needs to be made when clearing out an `<input type='file'/>` input in a [[developer/ReactJS/ReactJS\|ReactJS]] app. The easy way is to use the `useRef` hook.

```javascript
function ExampleFileInput() {
  const ref = React.useRef();
  function handleClick() {
    ref.current.value = ""
  }
  return (
    <div className="App">
      <input type="file" ref={ref}/><br />
      <button type="button" onClick={handleClick}>Clear file</button>
    </div>
  );
}
```

---
## Credits
- [link](https://stackoverflow.com/questions/60986168/react-how-to-clear-file-input-and-data-input-fields-after-submitting-form#:~:text=Here's an example functional component that does this%3A,%2F> <button type%3D"button" onClick%3D {handleClick}>Clear file<%2Fbutton> <%2Fdiv>)%3B })