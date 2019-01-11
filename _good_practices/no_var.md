---
title: No var
position: 2
---

**Ban `var`**. Default to `const` instead of [var](<https://eslint.org/docs/rules/no-var>).
```js
/* BAD */
var n = 1;

/* BETTER */
let n = 1;

/* BEST */
const n = 1;
```
```js
/* BAD */
for (var i = 0; i < n; i++)
  //

/* GOOD */
for (let i = 0; i < n; ++i)
  //
```
You can update your code by replacing every `var` with `const` and replace `const` with `let` when it fails (your linter will tell you).
