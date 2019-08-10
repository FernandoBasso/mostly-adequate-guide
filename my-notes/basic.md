
- [New Global Helpers vs Eslint](#new-global-helpers-vs-eslint)
- [reduce initial value](#reduce-initial-value)
- [eval scope of evaluated code](#eval-scope-of-evaluated-code)



## New Global Helpers vs Eslint

If you add a new function to `support.js`, some editors do not immediately understand there is a new global function. Reopening the editor solves the problem. Looks like that happens because eslint is run and eslintrc.js file is evaluated once, when opening the editor.

The `globals` generated below is an object with names of global functions as keys, and `true` as the value for each, so, eslint understands those are available global functions.

```js
const globals = Object
  .keys(require('./support'))
  .reduce((o, k) => ({ ...o, [k]: true }), { requirejs: true, assert: true });

module.exports = {
  extends: 'airbnb',
  env: {
    browser: true,
    amd: true,
  },
  globals,
  rules: {
    'import/no-amd': 0,
    'import/no-dynamic-require': 0,
    'no-unused-vars': 0,
    'object-curly-newline': [2, {
      multiline: true,
      consistent: true,
      minProperties: 5,
    }],
  },
};
```


<section class="qa">
<details>
<summary class="q">

## reduce initial value

QUESTION: What is the problem with both snippets below?

```js
[].reduce((previous, current) => {
  return previous + current;
});

''.split('').reduce((previous, current) => {
  return `${previous}-${current}`;
});
```
</summary>

<div class="a">

The problem is that with empty arrays and _no default initial values_ for `reduce`, the JS engine doesn't have anything to return and throws an exception.

TIP: Always provide an initial, default value for your reduces. 0 or 1 for reduces that sum or subtract, 1 for addition and subtraction, empty string for strings. For other cases, consider each one carefully.

ANSWER: Correct:

```js
[].reduce((previous, current) => {
  return previous + current;
}, 0);

''.split('').reduce((previous, current) => {
  return `${previous}-${current}`;
}, '');
```
</div>
</details>
</section>

--------------------------------------------------------------------------------

<section class='qa'>
<details>
<summary class='q'>

## eval scope of evaluated code

You `eval` a js file and want to use a function from the evaluated code, except the function is not in scope. What is the problem?

```js
// add.js
function add(x, y) {
  return x, y;
};
module.exports = { add };

// index.js
const fs = require('fs');
eval(fs.readFileSync('./add.js').toString('utf-8'));
console.log(add(2, 5));
// ReferenceError: add is not defined
```

</summary>
<div class='a'>

The “problem” is that evaluated code runs on its own scope. Stuff in that scope is inaccessible on the current scope. One must _concatenate_ the contents of `add.js` with the `console.log` thingy.

```js
// 1. Read add.js as string.
const addToEval = fs.readFileSync('./add.js').toString('utf-8');

// 2. Another string with code to log the addition.
const logToEval = 'console.log(add(2, 5));';

// 3. Concatenate both.
const codeToEval = addToEval + logToEval;

// 4. Then it works.
eval(codeToEval);
// 7

</div>
</details>
</section>
