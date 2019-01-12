---
title: No globals
position: 4
---

Do not needlessly pollute the global namespace.  
<https://eslint.org/docs/rules/no-global-assign>  
<https://eslint.org/docs/rules/no-implicit-globals>

In browsers
```js
/* BAD */
"use strict";
const nItems = 10;
// rest of your code here

/* GOOD */
"use strict";
(function () {
  const nItems = 10;
  // rest of your code here
})();

/* GOOD alternative */
"use strict";
{
const nItems = 10;
// rest of your code here
}
```

In modules or userscripts
```js
const nItems = 10;  // modules have their own scope so it's fine
// rest of your code here
```
