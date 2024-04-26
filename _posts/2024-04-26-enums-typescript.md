---
layout: post
title: "Enum Unexpected Behaviour Typescript"
excerpt: "Iterating over a list in typescript return twice as much data"
tags: [typescript, syntax]
comments: true
---

While working in a simple code that iterates over an Enum in typescript I found a weird tests failure.

```typescript
enum NotificationType {
  NewOffers = 1,
  ApplicationUpdates = 2,
}

const keys = Object.keys(NotificationType);

keys.forEach((key, index) => {
  console.log(`${key} has index ${index}`);
});
```

the example above will list 4 entries

```
"1 has index 0"
"2 has index 1"
"NewOffers has index 2"
"ApplicationUpdates has index 3"
```

## Iterating over values

I would it fascinating. In my test case, I just wanted to iterate over the values - then funny enough, but not using keys, I get only two entries.

```typescript
for (const notificationId of Object.values(NotificationType) as Array<number>) {
  console.log(`Value ${notificationId}`);
}
```

```
Value 1
Value 1
```
