## What is this?

> [!CAUTION]
> This is a repo testing the [TFG of Pablo Santana](https://riull.ull.es/xmlui/handle/915/43236).
> The syntax and the semantic of the proposed extension to JavaScript are described in the TFG. 
> The packages are published in the [ULL-ESIT-PL GitHub registry](https://github.com/orgs/ULL-ESIT-PL/packages).


This repo illustrates how to use 
the set of packages [published in the GitHub registry](https://github.com/orgs/ULL-ESIT-PL-2425/packages) inside the [ull-esit-pl-2425](https://github.com/ULL-ESIT-PL-2425/) organization. 
These packages extend the JavaScript language with a new kind of functions. The packages are:

- The JS parser modified: [@ull-esit-pl-2425/babel-parser](https://github.com/orgs/ULL-ESIT-PL-2425/packages/npm/package/babel-parser)
- The AST transformation plugin: [@ull-esit-pl-2425/babel-plugin-left-side ](https://github.com/orgs/ULL-ESIT-PL-2425/packages/npm/package/babel-plugin-left-side) 
- The support library: [@ull-esit-pl-2425/babel-plugin-left-side-support](https://github.com/orgs/ULL-ESIT-PL-2425/packages/npm/package/babel-plugin-left-side-support) 

### The proposed Syntax and Semantic

These packages extend JS  with a new kind of functions, the `@@` functions (we lack of a name for this class of functions: *assignable*? *pure*?). Here is an example of declaring an *assignable* function:

```js 
function @@ foo(bar) {
  return bar * 2;
}
```

These *assignable* functions can be later modified  using the assign expression:

```js
foo(10) = 5;
```

Here is the full code for our "hello" left-side-plugin example:

`➜  babel-npm-test git:(main) cat example.js`
```js
function @@ foo(bar) {
  return bar * 2;
}
foo(10) = 5;

console.log(foo(10)); //  5
console.log(foo(5));  // 10
```

You can fork this repo and test the packages in your own workspace.

## Install

Here are the node and npm versions I have used to test the packages:

```bash
➜  babel-left-side-npm-test git:(crguezl) ✗ node --version
v25.6.0
➜  babel-left-side-npm-test git:(crguezl) ✗ npm --version
11.12.1
```

These packages use the GitHub registry instead of the npm registry. Therefore, remember
to set the registry entry in your `.npmrc` file:

```bash
➜  babel-left-side-npm-test git:(2026) ✗ cat ~/.npmrc | grep '@ull-esit-pl:' 
@ull-esit-pl:registry=https://npm.pkg.github.com
```

If you set an entry `registry` in your `package.json` file that means this package will be published in the mentioned registry:

```bash
➜  babel-npm-test git:(main) ✗ jq '.registry' package.json 
"https://npm.pkg.github.com"
```

Then you can proceed to install the packages:

```
npm i   
```

Your package.json `devDependencies` section will look similar to this:

`➜  babel-left-side-npm-test git:(main) ✗ jq '.devDependencies' package.json`
```json
{
  "@babel/cli": "^7.10.1",
  "@ull-esit-pl/babel-plugin-left-side-plugin": "latest",
  "@ull-esit-pl/babel-plugin-left-side-support": "latest",
  "@ull-esit-pl/parser-left-side": "latest"
}
```


## Compiling the code

To compile the example above add a `babel.config.js` to your workspace folder:

`➜  babel-npm-test git:(main) cat babel.config.js`
```js
module.exports = {
  "plugins": [
    "@ull-esit-pl/babel-plugin-left-side-plugin"
  ],
}
```

and then compile it using the installed packages:

```js
npx babel  example.js
```
This will output the compiled code to the console:

```js                                                      
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

If you want to save it to a file, use the `-o` option.

## Running

You can pipe the output to `node`:

```bash
npx babel  example.js | node  -
```
This will output:
```
5
10
```

or alternatively, use the `-o` option to save the output to a file and then run it:

```
npx babel  example.js -o example.cjs
node example.cjs 
```
```
5
10
```

## References

- Our tutorial on babel: https://github.com/ULL-ESIT-PL/babel-learning/tree/main
- A tutorial in it describing how thhe packages are published: https://github.com/ULL-ESIT-PL/babel-learning/blob/main/doc/building-publishing.md
- Branch pablo-tfg with the actual code implementation: https://github.com/ULL-ESIT-PL/babel-tanhauhau/tree/pablo-tfg
- Some internal information: https://github.com/ULL-ESIT-PL/beca-colaboracion/tree/main
- The original idea of the project is based on what is explained in this draft: https://www.authorea.com/users/147476/articles/1235078-function-expressions-on-the-left-side-of-assignments (submitted now to Science of Computer Programming
 journal)

## License

[MIT](https://couto.mit-license.org/)