---
title: Basics of NPM
position: 0
---

1. <https://docs.npmjs.com/downloading-and-installing-node-js-and-npm>
2. <https://docs.npmjs.com/packages-and-modules/getting-packages-from-the-registry>
3. <https://docs.npmjs.com/cli/v8/commands/npx>
4. <https://docs.npmjs.com/cli/v8/using-npm/developers>
5. <https://docs.github.com/en/packages/working-with-a-github-packages-registry/working-with-the-npm-registry>

**Relevant info on a package page**  
If it's __deprecated__, don't use it.  
<https://imgur.com/dBN6gVc>  
  
Date of __Last publish__  
<https://imgur.com/PQJiC0G>  
A few months is generally fine.  
A few years either means
 - the package is very small and doesn't need to be updated because it does its job without issues
 - the package is unmaintained
Check the age of pull requests/issues on its Github to see if the author(s) are responsive.

Check if its **license** matches your needs (see License)

A small list of bad packages and their alternatives
 - `request`. Use `node-fetch`. Since node v18, fetch is native
 - `sqlite`, `sqlite3`. Use `better-sqlite3`
 - `mysql`. Use `mysql2` (via `mysql2/promise`) or `knex`
