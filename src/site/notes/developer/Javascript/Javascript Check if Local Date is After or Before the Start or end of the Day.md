---
{"dg-publish":true,"permalink":"/developer/javascript/javascript-check-if-local-date-is-after-or-before-the-start-or-end-of-the-day/","dgPassFrontmatter":true}
---

I needed a helper script that helps calculate days in a availablity calendar

1. converts a **UTC** date string to a local time string
2. calculate total minutes of the day used (seconds and milliseconds do not matter to me in this case)
3. Check if **start time** is at the top of the day (midnight)
4. Check if the **end time** goes past the last minute of the day '23:59'

```ts

const avail = {
	start: '2023-09-01T05:00:00.000Z',
	end: '2023-10-01T04:59:00.000Z',
}

const avail2 = {
	start: '2023-10-02T03:00:00.000Z',
	end: '2023-10-31T15:00:00.000Z',
}

const startLocal = new Date(startDate.getTime())
const startLocalMin = (startLocal.getHours() * 60) + startLocal.getMinutes()
const endLocal = new Date(endDate.getTime())
const endLocalMin = (endLocal.getHours() * 60) + endLocal.getMinutes()

console.log({startLocalMin});


if(startLocalMin > 0){
	console.table({
		startDate,
		startLocal,
		startLocalMin,
		message: 'begins later than midnight on this day'
	})
} else {
	// todo set startDate one day later
	console.table({
		startDate,
		startLocal,
		startLocalMin,
		message: 'starts at the top of the day'
	})
}


if(endLocalMin > 1439){
	console.table({
		endDate,
		endLocal,
		endLocalMin,
		message: 'rolls into the next day a 24 hour day'
	})
} else {
	console.table({
		endDate,
		endLocal,
		endLocalMin,
		message: 'ends on or before 23:59 of this day'
	})
}
```