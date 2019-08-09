# Mostly Adequate Exercises


## Overview

All exercises from the book can be completed either:

- in browser (using the version of the book published on [gitbook.io](https://mostly-adequate.gitbooks.io/mostly-adequate-guide/))
- in your editor & terminal, using `npm`

In every folder named `ch**` from this `exercises/` folder, you'll find three types of files:

- exercises
- solutions
- validations

Exercises are structured with a statement in comment, followed by an incomplete
or incorrect function. For example, `exercise_a` from `ch04` looks like this:


```js
// Refactor to remove all arguments by partially applying the function.

// words :: String -> [String]
const words = str => split(' ', str);
```

Following the statement, your goal is to refactor the given function `words`. Once done, 
your proposal can be verified by running:

```
npm run ch04
```

Alternatively, you can also have a peak at the corresponding solution file: in this case
`solution_a.js`. 

> The files `validation_*.js` aren't really part of the exercises but are used
> internally to verify your proposal and, offer hints when adequate. The curious 
> reader may have a look at them :).

Now go and learn some functional programming λ!

## Not Discarding Original Proposed Exercise Files

If one wants to write the solutions without losing the original `exercise_?.js` files, I made a small modification to this branch which allows one to test the solutions against a copy of the original file. Just copy and rename the `exercise_?.js` file to `exercises_?-answer.js` and run the exercises making sure the set `NODE_ENV` to `practice`. Example:

```shell
$ tree -C ch04/
# ch04/
# ├── exercise_a.js
# ├── exercise_b.js
# ├── exercise_c.js
# ├── solution_a.js
# ├── solution_b.js
# ├── solution_c.js
# ├── validation_a.js
# ├── validation_b.js
# └── validation_c.js

$ for file in ch04/exercise_?.js ; do cp -v "$file" "${file%*.js}-answer.js" ; done
# 'ch04/exercise_a.js' -> 'ch04/exercise_a-answer.js'
# 'ch04/exercise_b.js' -> 'ch04/exercise_b-answer.js'
# 'ch04/exercise_c.js' -> 'ch04/exercise_c-answer.js'
```

Then, try exercise "a" and test it:

```shell
vim ch04/exercise_a-answer.js
# Write your solution and save.

$ NODE_ENV=practice npm run ch04 
# > mocha test/ch04.js

#   Exercises Chapter 04
#     ✓ exercise_a-answer
#     1) exercise_b-answer
#     2) exercise_c-answer
#
#   1 passing (16ms)
#   2 failing
```

Try the others and test again:

```shell
$ NODE_ENV=practice npm run ch04 

#   Exercises Chapter 04
#     ✓ exercise_a-answer
#     ✓ exercise_b-answer
#     ✓ exercise_c-answer
#
#   3 passing (13ms)
```

Later, if you want to try the exercises again later, just run the for loop again. It will create fresh copies of the original exercises so you can practice more.


## About the Appendixes

Important notice: the exercise runner takes care of bringing all
data-structures and functions from the appendixes into scope. Therefore, you
may assume that any function present in the appendix is just available for you
to use! Amazing, isn't it? 
