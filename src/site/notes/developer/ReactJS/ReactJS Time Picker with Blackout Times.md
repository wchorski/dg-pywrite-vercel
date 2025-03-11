---
{"dg-publish":true,"tags":["javascript","reactjs","webdev"],"permalink":"/developer/ReactJS/ReactJS Time Picker with Blackout Times/","dgPassFrontmatter":true}
---

Very simple demo of how an input of 'blackout times' can disable options shown in an list of time options to pick from with [[developer/ReactJS/ReactJS\|ReactJS]]

- [codesandbox](https://codesandbox.io/s/date-compare-overlap-v30jrr?file=/src/components/TimePicker.tsx)

### BookingForm.tsx
```tsx
import { useState } from "react";
import { TimePicker } from "./TimePicker";

const blackoutTimes = [
  "00:00:00",
  "00:15:00",
  "00:30:00",
  "09:00:00",
  "13:30:00",
  "18:15:00"
];

export function BookingForm() {
  const [values, setValues] = useState({
    date: "",
    timeStart: "",
    timeEnd: ""
  });

  return (
    <form>
      <p>this is form</p>

      <TimePicker
        values={values}
        setValues={setValues}
        blackouts={blackoutTimes}
      />
    </form>
  );
}
```

### TimePicker.tsx
```tsx
import { timesArray } from "../lib/generateTimesArray";

interface iProps {
  values: any | undefined;
  setValues: any;
  blackouts: string[] | undefined;
}

type TimeOpt = {
  value: string;
  label: string;
};

// todo just start at 00:00:00 and have 15min incraments. then from there filter out times that don't work.
export function TimePicker({ values, setValues, blackouts }: iProps) {
  const listitemStyle = (i: number) => ({
    animationDelay: `calc(${i} * 0.03s)`
  });

  return (
    <ul className={values["date"] ? "open" : ""}>
      {timesArray().map((timeobj: TimeOpt, i) => (
        <li key={`time-${i}`} style={listitemStyle(i)}>
          <button
            type="button"
            className={timeobj.value === values.timeStart ? "active" : ""}
            onClick={() =>
              setValues((prev: any) => ({ ...prev, timeStart: timeobj.value }))
            }
            disabled={blackouts.includes(timeobj.value) ? true : false}
          >
            {timeobj.label} <br />
            {timeobj.value}
          </button>
        </li>
      ))}
    </ul>
  );
}
```

### generateTimesArray.js
```js
export function timesArray() {
  const startTime = "00:00";
  const endTime = "23:59";
  const timeInterval = 15;
  const options = [];

  const startDate = new Date(`2000-01-01T${startTime}:00`);
  const endDate = new Date(`2000-01-01T${endTime}:00`);

  for (
    let time = startDate;
    time <= endDate;
    time.setMinutes(time.getMinutes() + timeInterval)
  ) {
    options.push({
      label: time.toLocaleTimeString("en-US", {
        hour: "numeric",
        minute: "numeric",
        hour12: true
      }),
      value: time.toLocaleTimeString([], { hourCycle: "h23" })
      // value: time.toString()
    });
  }

  return options;
}
```