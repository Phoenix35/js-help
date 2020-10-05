---
title: No prototype builtins
position: 6
---

Do not assume a safe prototype. Do not create an intermediate object.  
<https://eslint.org/docs/rules/no-prototype-builtins>
```js
/* BAD */
someObj.hasOwnProperty("prop");  // Not safe
({}).hasOwnProperty.call(someObj, "prop");  // Intermediate empty object

/* GOOD */
Object.prototype.hasOwnProperty.call(someObj, "prop");  // Safe, one time use
// or
const _hasOwnProperty = Object.prototype.hasOwnProperty;  // Safe, several times use
_hasOwnProperty.call(someObj, "prop");
```
```js
/* BAD */
const arr = ([]).slice.call(iterable);  // Intermediate empty array

/* GOOD */
// If you don't map
const arr = [...iterable];
// If you map
const arr = Array.from(iterable, mapperFn);

// if array-like and not iterable
const arrayLikeObjLength = arrayLikeObj.length;
const arr = [];

for (let i = 0; i < arrayLikeObjLength; ++i)
  arr.push(arrayLikeObj[i]);
```
