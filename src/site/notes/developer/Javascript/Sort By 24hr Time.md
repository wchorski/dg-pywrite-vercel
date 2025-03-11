---
{"dg-publish":true,"permalink":"/developer/javascript/sort-by-24hr-time/","dgPassFrontmatter":true}
---

I have an Array of objects each with a 24 hour `time` attribute. I want to simply sort the array so earlier times `00:00` are put in the front and later times `23:55` are put at the end of the array before I map over. Here is a bit of [[developer/Javascript/Javascript\|Javascript]] to make the sort happen.

```jsx
{doc.sections
    .sort((prev, curr) =>{
      if(!prev.timeStart || !curr.timeStart) return

      if (prev.timeStart > curr.timeStart) return 1

      if (prev.timeStart < curr.timeStart) return -1

      return 0

    })
    .map((sec, i) => {

      if(!sec) return 

      console.log('sorted → ', sec.header, ' - ', sec.timeStart);

      return (

          <MyComponent 
            key={i}
            section={sec} 
          />
      )
    })}
```

## What If I want Times between midnight and 3am to be the next day?
I was using this snippet for my [[developer/Projects/Party-Planner\|Party-Planner]] app. I needed to account for "end of night" to sometimes be after midnight like 12:30am, 1am, etc. Those Dates needed to account for a time in the next day, not sort themselves to the top of the current day. 

Here is a little helper hack that makes it work. The `2024-01-01T` date is an arbitrary date just so I can programmatically make it the day later `2024-01-02T`

```ts
export function appendDateForSorting(time:string) {
  
  switch (true) {
	// you could omit this first case since the default does the same thing
    case time >= '04:00':
      return '2024-01-01T' + time
      
    case time < '03:59':
      return '2024-01-02T' + time
      
    default:
      return '2024-01-01T' + time
  }
}
```

Now update `.sort()`

```tsx
.sort((prev, curr) => {
	const a = appendDateForSorting(prev.timeStart)
	const b = appendDateForSorting(curr.timeStart)
	if (a > b) return 1
	if (a < b) return -1
	return 0

}).map(...)
```

---
## Credits 
- [Easily Sort Before Displaying Records in React JS | SUPER USEFUL - YouTube](https://www.youtube.com/watch?v=zZzcnmU_LoU)