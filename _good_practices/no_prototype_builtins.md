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
const arr = ([]).slice.call(arrayLikeObj);  // Intermediate empty array

/* GOOD */
// if iterable, e.g. a NodeList
const arr = Array.from(arrayLikeObj);
// if not
const arr = Array.prototype.slice.call(arrayLikeObj);
```
