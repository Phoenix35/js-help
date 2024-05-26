---
title: What is necessary in JavaScript.info?
position: 0
---

[JS.info](<https://javascript.info/>) is currently the best online resource out there to learn modern JS and some frontend, but mandatory entries are mixed with optional knowledge. Here's what you should take out of js.info

## [Part 1](<https://javascript.info/#tab-1>)

- [An introduction](<https://javascript.info/getting-started>): **All**
- [JavaScript fundamentals](<https://javascript.info/first-steps>): **All**
- [Code quality](<https://javascript.info/code-quality>): **3.1**; **3.2**; **3.3**
- [Objects: the basics](<https://javascript.info/object-basics>): **All**
- [Data types](<https://javascript.info/data-types>): **All but 5.8** (WeakMap and WeakSet)
- [Advanced working with functions](<https://javascript.info/advanced-functions>): **All but 6.7** (The "new Function" syntax)
- [Object properties configuration](<https://javascript.info/object-properties>): Optional
- [Prototypes, inheritance](<https://javascript.info/prototypes>): **All**
- [Classes](<https://javascript.info/classes>): **All**
- [Error handling](<https://javascript.info/error-handling>): **All**
- [Promises, async/await](<https://javascript.info/async>): **All but 11.3** (Promises chaining) **and 11.4** (Error handling with Promises)
- [Generators, advanced iteration](<https://javascript.info/generators-iterators>): Optional but may be encountered if dealing with [streams](<https://developer.mozilla.org/en-US/docs/Web/API/ReadableStream#async_iteration_of_a_stream_using_for_await...of>)
- [Modules](<https://javascript.info/modules>): **All**
- [Miscellaneous](<https://javascript.info/js-misc>): **14.4** [Reference Type](<(<https://javascript.info/reference-type>)>)

## [Part 2](<https://javascript.info/#tab-2>)

Even if you won't work with the browser, there are concepts revelant for all environments

- [Introduction to Events](<https://javascript.info/events>): **2.1** [Introduction to browser events](<https://javascript.info/introduction-browser-events#addeventlistener>)
- [Miscellaneous](<https://javascript.info/ui-misc>): **6.3** [Event loop: microtasks and macrotasks](<https://javascript.info/event-loop>)

If browser environment: **5.2** [Scripts: async, defer](<https://javascript.info/script-async-defer>) see related **[correct use of JS in the browser](<https://phoenix35.js.org/good-practices.html#correct-use-of-js-in-the-browser>)**; the entirety of Part 2

## [Part 3](<https://javascript.info/#tab-3>)

- [Binary data, files](<https://javascript.info/binary>): **2.3** [Blob](<https://javascript.info/blob>)
- [Network requests](<https://javascript.info/network>): **3.1** [Fetch](<https://javascript.info/fetch>); **3.5** [Fetch: Cross-Origin Requests](<https://javascript.info/fetch-crossorigin)<
- [Regular expressions](<https://javascript.info/regular-expressions>): **All**. But never use RegExp to parse HTML
