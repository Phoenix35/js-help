---
title: From promises to async functions
position: 0
---

(WIP) By example
```js
function fetchJSON (url) {
  return fetch(url).then(response => response.json());
}

function updatePara (url) {
  fetchJSON(url).then(data => {
    const { title, text }  = data;

    paragraph.dataset.title = title;
    paragraph.textContent = text;
  })
  .catch(log);
}
```
```js
async function fetchJSON (url) {
  const response = await fetch(url);

  return response.json();
}

async function updatePara (url) { // You obviously NEED the async keyword
  try {
    const { title, text } = await fetchJSON(url);

    paragraph.dataset.title = title;
    paragraph.textContent = text;
  } catch (err) {
    log(err);
  }
}
```

<https://2ality.com/2016/10/async-function-tips.html>
