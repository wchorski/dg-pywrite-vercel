---
{"dg-publish":true,"permalink":"/developer/ReactJS/ReactJS Date Time Range Overlap Checker/","dgPassFrontmatter":true}
---

making this, because wrapping your head around dates is mind boggling

[codesandbox](https://codesandbox.io/s/date-compare-overlap-v30jrr?file=/src/App.js)

I've even included little text diagrams that show visually how the times overlap

```bash
range1.end overlaps range2.start
|----|
...|----|

range1.start overlaps range2.end
...|----|
|----|

range1 is inside range2
...|--|...
|--------|

range2 is inside range1
|------|
...|-|...

range1.end overlaps range2.start
|----|
...|----|


no overlap
|----|.......
........|----|
```

```jsx
import "./styles.css";

const gig = {
  start: "2023-05-10T13:30:00",
  end: "2023-05-10T15:00:00"
};
const busy1 = {
  start: "2023-05-10T14:00:00",
  end: "2023-05-10T16:00:00"
};
const busy2 = {
  start: "2023-05-10T9:00:00",
  end: "2023-05-10T14:00:00"
};
const busy3 = {
  start: "2023-05-09T14:00:00",
  end: "2023-05-11T16:00:00"
};
const busy4 = {
  start: "2023-04-10T14:00:00",
  end: "2023-06-10T14:30:00"
};
const busy5 = {
  start: "2023-08-10T14:00:00",
  end: "2023-08-10T16:00:00"
};

export default function App() {
  return (
    <div className="App">
      <h1>Date Overlap Checker</h1>

      <h3> 1. Gig date bleeds into busy range</h3>
      <code>
        |-----| <br />
        ......|-----|
      </code>

      <table>
        <tr>
          <td>gig start: </td>
          <td>{datePretty(gig.start)}</td>
        </tr>
        <tr>
          <td>gig end: </td>
          <td>{datePretty(gig.end)}</td>
        </tr>
        <tr>
          <td>busy start: </td>
          <td>{datePretty(busy1.start)}</td>
        </tr>
        <tr>
          <td>busy end: </td>
          <td>{datePretty(busy1.end)}</td>
        </tr>
      </table>

      <p>
        {dateRangesOverlap(gig, busy1)
          ? "DATES OVERLAP"
          : "dates do not overlap"}
      </p>
      <hr />

      <h3> 2. Busy End lands inside Gig range</h3>
      <code>
        |-----| <br />
        |-----|....
      </code>

      <table>
        <tr>
          <td>gig start: </td>
          <td>{datePretty(gig.start)}</td>
        </tr>
        <tr>
          <td>gig end: </td>
          <td>{datePretty(gig.end)}</td>
        </tr>
        <tr>
          <td>busy start: </td>
          <td>{datePretty(busy2.start)}</td>
        </tr>
        <tr>
          <td>busy end: </td>
          <td>{datePretty(busy2.end)}</td>
        </tr>
      </table>

      <p>
        {dateRangesOverlap(gig, busy2)
          ? "DATES OVERLAP"
          : "dates do not overlap"}
      </p>
      <hr />

      <h3> 3. Gig range lands inside Busy Range</h3>
      <code>
        |-----| <br />
        |-------------|
      </code>

      <table>
        <tr>
          <td>gig start: </td>
          <td>{datePretty(gig.start)}</td>
        </tr>
        <tr>
          <td>gig end: </td>
          <td>{datePretty(gig.end)}</td>
        </tr>
        <tr>
          <td>busy start: </td>
          <td>{datePretty(busy3.start)}</td>
        </tr>
        <tr>
          <td>busy end: </td>
          <td>{datePretty(busy3.end)}</td>
        </tr>
      </table>

      <p>
        {dateRangesOverlap(gig, busy3)
          ? "DATES OVERLAP"
          : "dates do not overlap"}
      </p>

      <hr />

      <h3> 4. Busy range lands inside gig</h3>
      <code>
        |----------| <br />
        |------|
      </code>

      <table>
        <tr>
          <td>gig start: </td>
          <td>{datePretty(gig.start)}</td>
        </tr>
        <tr>
          <td>gig end: </td>
          <td>{datePretty(gig.end)}</td>
        </tr>
        <tr>
          <td>busy start: </td>
          <td>{datePretty(busy4.start)}</td>
        </tr>
        <tr>
          <td>busy end: </td>
          <td>{datePretty(busy4.end)}</td>
        </tr>
      </table>

      <p>
        {dateRangesOverlap(gig, busy4)
          ? "DATES OVERLAP"
          : "dates do not overlap"}
      </p>

      <hr />

      <h3> 5. No overlap</h3>
      <code>
        |-----|........
        <br />
        .........|------|
      </code>

      <table>
        <tr>
          <td>gig start: </td>
          <td>{datePretty(gig.start)}</td>
        </tr>
        <tr>
          <td>gig end: </td>
          <td>{datePretty(gig.end)}</td>
        </tr>
        <tr>
          <td>busy start: </td>
          <td>{datePretty(busy5.start)}</td>
        </tr>
        <tr>
          <td>busy end: </td>
          <td>{datePretty(busy5.end)}</td>
        </tr>
      </table>

      <p>
        {dateRangesOverlap(gig, busy5)
          ? "DATES OVERLAP"
          : "dates do not overlap"}
      </p>

      <hr />
    </div>
  );
}

function dateRangesOverlap(gig, busy) {
  const gigStart = new Date(gig.start).getTime();
  const gigEnd = new Date(gig.end).getTime();
  const busyStart = new Date(busy.start).getTime();
  const busyEnd = new Date(busy.end).getTime();

  // Check if range1 overlaps with range2
  if (gigStart <= busyEnd && gigEnd >= busyStart) {
    return true;
  }

  // Check if range2 overlaps with range1
  if (busyStart <= gigEnd && busyEnd >= gigStart) {
    return true;
  }

  // If there is no overlap, return false
  return false;
}

function datePretty(date) {
  const options = {
    year: "numeric",
    month: "long",
    day: "numeric",
    timeZone: "America/Chicago",
    timeZoneName: "short",
    hour: "numeric",
    minute: "numeric",
    hour12: true
  };

  const newDate = new Date(date);

  return newDate.toLocaleTimeString("en-US", options);
}

```

[[developer/Home Lab/ReactJS\|ReactJS]]