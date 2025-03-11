---
{"dg-publish":true,"permalink":"/developer/react-js/array-of-react-refs/","dgPassFrontmatter":true}
---

Need to make an array of refs with the `useRef` hook. So how do I do add them programmatically inside `map` or `foreach`

```jsx
import React, {useRef} from 'react'

export const App = ( {doc} ) => {
	const childrenRef = useRef([]);
	childrenRef.current = []
	
	const handleRef = (el) => {
	    if(el && !childrenRef.current.includes(el)){
	      childrenRef.current.push(el)
	    }
	    console.log('childrenRefs.current', childrenRef.current);
	  }
	
	return(
		{doc.sections.map((item, i) => {
	      if(!item.header) return null
	      return <p key={i} ref={handleRef}>{item.header}</p>
	    })}
	)
}
```

---
## Credits
- [How to create an array of React refs - YouTube](https://www.youtube.com/watch?v=ygPIjzhKB2s)
- [[developer/ReactJS/ReactJS\|ReactJS]]