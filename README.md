## What is this?

Testing Pablo's [@ull-esit-pl/parser-left-side](https://github.com/orgs/ULL-ESIT-PL/packages/npm/package/parser-left-side) set of packages [published in the GitHub registry](https://github.com/orgs/ULL-ESIT-PL/packages) inside the [ull-esit-pl](https://github.com/ULL-ESIT-PL/) organization.

### References

- https://github.com/ULL-ESIT-PL/babel-tanhauhau/tree/pablo-tfg
- https://github.com/ULL-ESIT-PL/babel-learning/tree/main
- https://github.com/ULL-ESIT-PL/beca-colaboracion/tree/main



## TODO

-  README.md is the one babel has. It has to change to be specific about the extension for the three packages,
-  Add `-D` to all install instructions and remove the version number: `npm install @ull-esit-pl/babel-plugin-left-side-plugin -D`
-  Do we need two separated packages for the plugin and the support? Can we have a single package?

## Node and npm versions

```bash
➜  babel-npm-test node --version
v20.5.0
➜  babel-npm-test npm --version
9.8.0
```

## Install

```
npm i -D @babel/cli@7.10 @ull-esit-pl/babel-plugin-left-side-plugin @ull-esit-pl/babel-plugin-left-side-support @ull-esit-pl/parser-left-side 
```

Your package.json `devDependencies` section will look similar to this:

`➜  babel-npm-test jq '.devDependencies' package.json`
```json
{
  "@babel/cli": "^7.10.1",
  "@ull-esit-pl/babel-plugin-left-side-plugin": "^1.0.1",
  "@ull-esit-pl/babel-plugin-left-side-support": "^1.0.0",
  "@ull-esit-pl/parser-left-side": "^1.0.0"
}
```

### Registry configuration

```bash
➜  babel-npm-test git:(main) ✗ cat ~/.npmrc | grep '@ull-esit-pl:'
@ull-esit-pl:registry=https://npm.pkg.github.com
```

```bash
➜  babel-npm-test git:(main) ✗ jq '.registry' package.json 
"https://npm.pkg.github.com"
```

## Usage

### Syntax

This package extends JS  with a new kind of functions, the `@@` functions (we lack of a name for this class of functions: *assignable*? *pure*?):

```js 
function @@ foo(bar) {
  return bar * 2;
}
```

These *assignable* functions can be later modified  using the assign expression:

```js
foo(10) = 5;
```

Here is the full code for the "hello left-side-plugin" example:

```js
➜  babel-npm-test git:(main) cat example.js 
function @@ foo(bar) {
  return bar * 2;
}
foo(10) = 5;

console.log(foo(10)); //  5
console.log(foo(5));  // 10
```

To compile it add a `babel.config.js` to your workspace folder:

`➜  babel-npm-test git:(main) cat babel.config.js`
```js
module.exports = {
  "plugins": [
    "@ull-esit-pl/babel-plugin-left-side-plugin"
  ],
}
```

and then used the installed packages:

```js
➜  babel-npm-test npx babel  example.js                                                      
const {
  assign,
  functionObject
} = require("@ull-esit-pl/babel-plugin-left-side-support");
const foo = functionObject(function (bar) {
  return bar * 2;
});
assign(foo, [10], 5);
console.log(foo(10));
console.log(foo(5));
```

## Running

```bash
➜  babel-npm-test npx babel  example.js | node  -
5
10
```

or alternatively:

```
➜  babel-left-side-npm-test git:(main) npx babel  example.js -o example.cjs
➜  babel-left-side-npm-test git:(main) ✗ node example.cjs 
5
10
```