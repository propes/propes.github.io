---
layout: post
title: "Javascript dates gotcha"
date: 2025-01-15 18:00:00 +1000
tag: javascript
---

When you create the same date two different ways in javascript you get subtly different results. Let's look at two different ways to create the date 14 Jan 2025 in a browser console.

```js
> new Date("2025-01-14")
<- Tue Jan 14 2025 10:00:00 GMT+1000 (Australian Eastern Standard Time) 

> new Date(2025, 0, 14) // Month is zero-indexed
<- Tue Jan 14 2025 00:00:00 GMT+1000 (Australian Eastern Standard Time)
```

Both are printed in the browser console as local dates but the first one represents 10am local time, or UTC midnight, and the second one represents midnight local time, or 13 Jan 2025 14:00 UTC. Therefore, comparing dates created these two different ways could catch you out because Date 1 != Date 2.

What's more, client generation tools like [Autorest](https://github.com/Azure/autorest) deserialize date strings to javascript `Date` objects for you and you need to be aware of how they deserialize that date. E.g. autorest uses the first method, so a date with no timestamp in an api payload such as "2025-01-14" will be converted to a UTC midnight date.

Let's imagine a couple of scenarios where this could catch you out...

1. Determining if the date is today's date

If I want to determine if the date I've received from an autorest payload is today's date I can't just compare it against `Date()` because this will give me the date in local time.

I need to ensure both dates are in utc time before comparing them. Example using `dayjs`:

```js
const apiDate = new Date("2025-01-14");
const apiDateUtcMidnight = dayjs(apiDate);
const todayUtcMidnight = dayjs().utc().startOf("day");

const isToday = apiDateUtcMidnight.isSame(todayUtcMidnight);
console.log("isToday", isToday);
```

2. Comparing against dates from a form control

Sometimes controls like DatePickers will return a `Date` object rather than a date string. If the `Date` object has been created via something like `new Date(2025, 0, 14)` then it will have a timestamp of mignight local time you have the same problem when comparing against a deserialized api date with has a midnight UTC timestamp. To compare like with like you have to convert one to the other first.

Example using `dayjs` to convert the local midnight date to UTC midnight:

```js
const toUTCMidnight = (localDate) => {
  const date = new Date(localDate);
  const utcDate = Date.UTC(date.getFullYear(), date.getMonth(), date.getDate(), 0, 0, 0);
  return new Date(utcDate);
};

const apiDate = new Date("2025-01-14");

const datePickerDate = new Date(2025, 0, 14);
const datePickerUtcMidnight = toUTCMidnight(datePickerDate);

const areSameDay = dayjs(apiDate).isSame(dayjs(datePickerUtcMidnight));
console.log("areSameDay", areSameDay);
```