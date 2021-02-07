---
title: Strict mode
position: 001
---

**This is not arguable**! Code in [strict mode](<https://eslint.org/docs/rules/strict>), ALWAYS!  
[What is strict mode?](<https://javascript.info/strict-mode>)

Every JS file in the **browser** that is not an [ES6 module](<https://javascript.info/modules>) MUST be written like this.
```js
// Use async if you intend to use top-level await
(/* async */ () => {
"use strict";

// rest of your code here
)();
```
ECMAScript Modules are already in strict mode so there is no need for the II(A)FE nor this directive.

In **node.js**, the II(A)FE is not needed but strict mode still is.  
For a file to be considered a module it must have the `.mjs` extension (note the **m**), or, project-wide, the `package.json` must contain `"type": "module"`.  
In any other case, you MUST indicate strict mode.
```js
"use strict";

// your requires

// rest of your code here
```
See the [ESM doc](<https://nodejs.org/api/esm.html#esm_modules_ecmascript_modules>) on node.js for more info.
