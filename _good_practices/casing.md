---
title: Casing
position: 3
---

Use [camelCase](<https://eslint.org/docs/rules/camelcase>) convention for all variables that are not constructors.  
Use [PascalCase](<https://eslint.org/docs/rules/new-cap>) convention for all constructors.  
You may UPPERCASE some constants.
```js
function Item (category) { // this is a constructor
  this.category = category;
  return this;
}
function createItem (category = "") { // this is not
  return new Item(category); 
}

const someItem1 = createItem("cat1");
const someItem2 = new Item("cat2");
```
