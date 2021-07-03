---
title: I cannot build a package on Windows
position: 0
---

You're on Windows and a package that has a build step (`node-canvas`, `bson`, `sqlite3`, etc...) outputs an error with a trace like  
`node-pre-gyp ERR! build error`  
?  
You need specific build tools on Windows that don't come natively and that node-gyp failed to install by itself.
  
Here are the quick steps to solve this:
1. Install [python](<https://www.python.org/downloads/>) (Python 3.9.6 at the time of writing)
1. You will now have to do some commands. From an elevated terminal (so with admin rights)  
    `npm i -g --production windows-build-tools --vs2015`  
    Don't worry, it's the [official build tools](<https://github.com/Microsoft/nodejs-guidelines/blob/master/windows-environment.md#environment-setup-and-configuration>)
1. When installed (it may take a while), from the same elevated prompt  
    `npm config set msvs_version 2015`
1. `where python`  
    Copy the resulting path and use it in the next command
1. `npm config set python C:\\Python39\\python.exe` (or your actual path, you get it)


Only do these steps this one time and you shouldn't have anymore problems
