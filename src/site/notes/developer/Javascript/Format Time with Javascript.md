---
{"dg-publish":true,"permalink":"/developer/javascript/format-time-with-javascript/","dgPassFrontmatter":true}
---

Use [[developer/Javascript/Javascript\|Javascript]]'s built in functions to format time. You could even dynamically show time depending on locale / region of the User. 

This could remove the need for #3rd-party libraries like [date-fns](https://date-fns.org/)or [Moment.js](https://momentjs.com/)

```js
const timeString = '12:00:00'

const timeString12hr = new Date('2024-01-01T' + timeString + 'Z')
  .toLocaleTimeString(
	  'en-US',
	{
	    timeZone:'UTC',
	    hour12:true,
	    hour:'numeric',
	    minute:'numeric'
	}
  );
```

## Credits
- [Javascript: convert 24-hour time-of-day string to 12-hour time with AM/PM and no timezone - Stack Overflow](https://stackoverflow.com/questions/13898423/javascript-convert-24-hour-time-of-day-string-to-12-hour-time-with-am-pm-and-no)