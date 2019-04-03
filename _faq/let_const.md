---
title: const or let?
position: 0
---

[`const`](<https://eslint.org/docs/rules/prefer%2Dconst>) always. Refer to [best practices](<{{ "/good-practices.html#no-var" | absolute-url }}>).  

By using `const`, you're stating: "This variable will only allowed to be an Enum (constant string, magic number, config boolean) or a collection (array, object, set, map) and nothing else, my linter will tell me if I screw up when I use it in any other way".  
Moreover, if you default to `const`, then `let` conveys more meaning: "This variable MUST change in the flow of my program (concatenate strings, add numbers, toggle a boolean dynamically, allow this object to be emptied again or extending it via spread)".

An added benefit of `const` is that `let` doesn't allow for [constant folding](<https://en.wikipedia.org/wiki/Constant_folding>).
