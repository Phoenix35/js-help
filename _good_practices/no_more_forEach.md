---
title: No more forEach
position: 5
---

**Ban `.forEach()`**! It only comes with downsides.

- Lack of control flow.  
  `continue;` can be simulated by `return;`, but `break` is not possible
- Not async friendly.  
  ```js
  let counter = 0;
  const nums = [ 2, 8, 10 ];
  nums.forEach(async (num) => {
    await something();
    counter++;
  });
  console.log(counter); // 0
  ```  
  ```js
  const urls = [ "https://remote.com/1", "https://remote.com/2", "https://remote.com/3", ... ];
  urls.forEach(async (url) => {
    const response = await fetch(url); // This is doing N CONCURRENT requests, you are spamming the server
    doStuff(response);
  });
  ```
- Looks like functional programming, while it is anything but.  
  The **purpose** of `.forEach` is **to have** side-effects
- Doesn't get to the point.  
  All the other Higher-Order Functions (HOFs) tell you what they do in their names. `.forEach` does everything so nothing
- Lends itself to misuse.  
  - `return something;` doesn't work.
  - People may want to chain it, like the other HOFs.
  - `async`ing the callback will bite you
- The only time you _would_ have a reason to use `.forEach` – when you want a side-effect – it is superseded by `for-of`.  
  It is compatible with any iterable instead of being a method of a select few, and has none of the downsides listed above

## Act on value only
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
// if values of an object
for (const value of Object.values(obj)) {
  
}
```

## Act on index/key only
```js
/* BAD */
iterable.forEach((_, key) => {
  
});

/* GOOD */
for (const key of iterable.keys()) {
  
}
// if keys of an object
for (const key of Object.keys(obj)) {
  
}
```

## Act on both key and value
```js
/* BAD */
iterable.forEach((value, key) => {
  
});

/* GOOD */
for (const [ key, value ] of iterable.entries()) {
  
}
// if an object
for (const [ key, value ] of Object.entries(obj)) {
  
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
`.map` Also called a transform, or a mapping operation.
```js
/* BAD */
const oldPlus2 = [];

oldArr.forEach((number) => {
  oldPlus2.push(number + 2);
});

/* GOOD */
const oldPlus2 = oldArr.map((number) => number + 2);
```

### Select entries
`.filter()` If you want a new array with entries selected by a predicate.
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
`.some()` To check if whether at least one element (early exit) in the array passes the predicate.  
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
