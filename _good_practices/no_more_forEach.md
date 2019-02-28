---
title: No more forEach
position: 5
---

**Ban `.forEach()`**! There are, [several](<https://gist.github.com/Phoenix35/bf6adbef90cdbc401f19920d1b97a00b#file-for-loops-js-L42>), [rationale](<https://jeremyliberman.com/2019/02/14/the-pitfalls-of-enumerating-with-foreach.html>) for this.

## Value only
```js
/* BAD */
iterable.forEach((value) => {
  
});

/* GOOD */
for (const value of iterable) {
  
}
// or
for (const value of iterable.values()) {
  
}
// or
for (const value of Object.values(obj)) {
  
}
```

## Key only
```js
/* BAD */
iterable.forEach((_, key) => {
  
});

/* GOOD */
for (const key of iterable.keys()) {
  
}
// or
for (const key of Object.keys(obj)) {
  
}
```

## Key and value
```js
/* BAD */
iterable.forEach((value, key) => {
  
});

/* GOOD */
for (const [key, value] of iterable.entries()) {
  
}
// or
for (const [key, value] of Object.entries(obj)) {
  
}
```

## The whole functionality
```js
/* BAD */
iterable.forEach(cb, thisArg);

/* GOOD */
for (const [key, value] of iterable.entries()) {  // adapt to whatever you actually need
  cb.call(thisArg, value, key, iterable);
}
```

## Higher-order functions
Eloquent JavaScript devoted [an entire chapter](<https://eloquentjavascript.net/05_higher_order.html>) to them.  
We're going to see what can be used instead of the dreaded forEach.

### Output an array the same length as the starting array
Also called a transform, or a mapping operation. Well, JavaScript has `.map()` to do exactly this.
```js
/* BAD */
const oldPlus2 = [];

oldArr.forEach((number) => {
  oldPlus2.push(number + 2);
});

/* GOOD */
const oldPlus2 = oldArr.map((number) => number + 2);
```

### Filter entries
`.filter()` If you want a new array that selects entries based on a condition.
```js
/* BAD */
const onlyEvens = [];

oldArr.forEach((integer) => {
  if (integer % 2 === 0)
    onlyEvens.push(onlyEvens);
});

/* GOOD */
const onlyEvens = oldArr.filter((integer) => integer % 2 === 0);
```

### Check if a condition is met by _at least_ one entry
`.some()` To check if whether at least one element (early exit) in the array passes the test implemented by the provided function.  
**/!\ This example is a tad more complicated /!\\**
```js
const authzCreds = [
  {
    id: 11,
    pubKey: "3a",
  },
  {
    id: 16,
    pubKey: "b0",
  },
  {
    id: 99,
    pubKey: "66",
  }
];

const group = [
  {
    id: 11,
    name: "Jack",
    pubKey: "66",  // pubKey will not pass
  },
  {
    id: 99,
    name: "James",
    pubKey: "66",  // You good
  },
  {
    id: 13,
    name: "Rose",
    pubKey: "3a",  // ID will not pass
  },
];

/* BAD */
// This is executed for EVERY element!
// We have to have a supplementary state in case we only need to proceed once
let hasExecuted = false;
group.forEach(({ id: personId, pubKey: personPubKey }) => {
  if (hasExecuted)
    return;

  let isAllowed = false;
  authzCreds.forEach((cred) => {
    if (personId === cred.id && personPubKey === cred.pubKey)
      isAllowed = true;
  });

  if (isAllowed) {  // Proceed only once.
    proceed();
    hasExecuted = true;
  }
});

/* GOOD */
// Loop can stop as soon as the condition is met
for (const { id, pubKey } of group) {
  if (
    authzCreds.some((cred) => id === cred.id && pubKey === cred.pubKey)
  ) {
    proceed();
    break;
  }
}

/* GOOD alternative */
for (const { id, pubKey } of group) {
  if (
    !authzCreds.some((cred) => id === cred.id && pubKey === cred.pubKey)
  )
    continue;
  proceed();
  break;
}
```

### Check if a condition is met _by all_ entries
`.every()` Early exit if one does not meet.
```js
/* WHAT EVEN? */
let canContinue = true;
onlyEvens.forEach((n) => {
  if (n % 2 !== 0)
    canContinue = false;
});
if (canContinue)
  proceed();

/* GOOD */
if (onlyEvens.every((n) => n % 2 === 0))
  proceed();
```

### Produce a single value out of an array
`.reduce()`
```js
const someBytes = new Uint8Array([0x10, 0xFF, 0x30, 0x00, 0x10]);
const bLength = BigInt(someBytes.BYTES_PER_ELEMENT * 8);

/* BAD */
let newBInt = 0n;
someBytes.forEach((currUint) => {
  newBInt = (newBInt << bLength) + BigInt(currUint);
});


/* GOOD */
const newBInt = someBytes.reduce(
  (accBigInt, currUint) => (accBigInt << bLength) + BigInt(currUint),
  0n
);
newBInt; // 0x10FF300010n === 73000812560n;
```

### Find the first _index_ meeting a certain condition
`.findIndex()` returns `-1` if not found, like its brother `indexOf`
```js
const fibNumbers = [1, 1, 2, 3, 5, 8, 13, 21];

console.log(`The first Fibonacci number above 10 is #${
  fibNumbers.findIndex((number) => number > 10) + 1  // 7
}`);
```

### Find the first _value_ meeting a certain condition
`.find()` returns `undefined` if the test is not passed. It returns a **reference to** the object, not a copy!
```js
const remoteDB = [
  {
    name: "B",
    version: 3,
  },
  {
    name: "A",
    version: 0,
  },
];

const localDB = [
  {
    name: "A",
    description: "Perspiciatis saepe sunt qui quo molestiae rerum.",
    version: 1,
  },
  {
    name: "B",
    description: "Blanditiis architecto sed est facilis qui.",
    version: 1,
  },
];

function updateLocalVersionFrom (db) {
  for (const newEntry of db) {
    const localEntry = localDB.find(({ name }) => name === newEntry.name);  // By reference!
    if (localEntry.version < newEntry.version)
      localEntry.version = newEntry.version;
  }
}

updateLocalVersionFrom(remoteDB);
// 0: Object { name: "A", ..., version: 1 }
// 1: Object { name: "B", ..., version: 3 }
```

---

## Summary
Sharp minds might have noticed that all higher-order functions can actually be written with `for...of` inside blocks instead.  
In my opinion, intent can usually be better expressed with higher-order functions than with loops.  
Use forEach if you're experienced enough and absolutely know what you're doing.

If you're returning something in a forEach callback, you don't know what you're doing.  
If you're chaining forEach after map or filter, you don't know what you're doing; reduce is an ambiguous case, you most likely don't know what you're doing.  
If you're using an async callback function, you most likely don't know what you're doing (see rationale).
