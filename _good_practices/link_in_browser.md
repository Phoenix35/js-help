---
title: Correct use of JS in the browser
position: 0
---

First, your `<script>` tag MUST be in the `<head>` part, and MUST NOT be inline. Ideally, make it an ES module.  
This allows:
- earliest possible download by the browser (`<head>`)
- download in parallel with the rest of the page.  
  If inline, the HTML bytes are downloaded and rendered later, because the browser has to parse JS in the same thread before
- parsing at the correct time (after your DOM is loaded)

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Amazing title</title>
  <link rel=stylesheet href="path/to/sheet.css">

  <!-- IDEALLY -->
  <script type=module src="path/to/module.js"></script>

  <!-- OR -->
  <script defer src="path/to/script.js"></script>

</head>
<body>

<!-- DO NOT -->
<script src="path/to/script.js"></script>

<!-- NEVER -->
<script>
/* JS code inside HTML */
</script>

</body>
</html>
```

Second, your HTML MUST NOT contain `on*` attributes. Simply because if you follow point #1, it doesn't work.  
Modules have their own scope, so the listener would be invisible to HTML

```html
<!-- BAD -->
<button onclick="submitForm();">Submit</button>
```
```js
// module.js

const myButton = document.querySelector("button");

// Not in the scope of HTML.
// An error will trigger on button click to say it's undefined
function submitForm (evt) {

}
```
IF you use regular scripts with the `defer` attribute in the `<script>` tag, the best practice is to prevent globals with an II(A)FE.
```js
// script.js
(/* async */ () => {
"use strict";

const myButton = document.querySelector("button");

// Same problem. Not in scope.
function submitForm (evt) {

}
})();
```
Use `.addEventListener` in JS instead
```html
<!-- GOOD -->
<button class="submit-button">Submit</button>
<!-- or any other way to identify the element -->
```
```js
const myButton = document.querySelector(".submit-button"); // change the selector accordingly

function submitForm (evt) {

}

myButton.addEventListener("click", submitForm, options);

// OR an anonymous function works too if you don't intend to reuse it
myButton.addEventListener("click", (evt) => {

}, options);
```
The `options` argument is detailed [here](<https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/addEventListener#parameters>).  
There are [several](<https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/addEventListener#why_use_addeventlistener>) reasons to [use](<https://javascript.info/introduction-browser-events#addeventlistener>) `.addEventListener`.
