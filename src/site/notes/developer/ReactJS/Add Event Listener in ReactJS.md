---
{"dg-publish":true,"permalink":"/developer/ReactJS/Add Event Listener in ReactJS/","dgPassFrontmatter":true}
---

I was having trouble with the `onLoadedMetadata` not firing when linked inside the `<audio>` tag. So hooking up an event listener 'the old fashion way' seemed to do the trick.

There are a couple things missing in this `Component.jsx`, but if you know how to structure a react component it should be easy to pick out the parts you need

```jsx

	function testClick() {
	
		console.log('test click');
	
	}
	
	useEffect(() => {
	
		document.getElementById("btn-test").addEventListener("click", testClick )
		
		return () => {
			document.getElementById("btn-test").removeEventListener("click", testClick )
		}
	
	}, [])

return (
	<button id="btn-test">test butn</button>
)
```

---
## Credits
- [javascript - Using useEffect with event listeners - Stack Overflow](https://stackoverflow.com/questions/55262596/using-useeffect-with-event-listeners)

## Backlinks
- [[developer/ReactJS/ReactJS\|ReactJS]]