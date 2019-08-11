# Ch04 Exercise Notes

- [Ch04 Exercise Notes](#ch04-exercise-notes)
  - [Exercise A, split](#exercise-a-split)
    - [Solution](#solution)
    - [Examples](#examples)
  - [Exercise B, filter](#exercise-b-filter)
    - [Solution 1 (incorrect)](#solution-1-incorrect)
    - [Solution 2 (correct)](#solution-2-correct)
    - [Examples](#examples-1)
  - [Exercise C, max](#exercise-c-max)
    - [Solution](#solution-1)
    - [Examples](#examples-2)


## Exercise A, split

Refactor to remove all arguments by partially applying the function.

```js
// words :: String -> [String]
const words = str => split(' ', str);
```

### Solution

We partially apply `split` by passing it only its first argument, which will cause a new function to be returned and assigned to `words`. Then, `words` is a function that remembers the first parameter provided to `split` and takes the remaining parameter, the input to be split.

```js
// words :: String -> [String]
const words = split(' ');
```

### Examples

```js
words('');
// [ '' ]

words('may the force be with you'));
// [ 'may', 'the', 'force', 'be', 'with', 'you' ]
```


## Exercise B, filter

Refactor to remove all arguments by partially applying the functions.

```js
// filterQs :: [String] -> [String]
const filterQs = xs => filter(x => x.match(/q/i), xs);
```

### Solution 1 (incorrect)

```js
// filterQs :: [String] -> [String]
const filterQs = filter(x => x.match(/q/i));
```
It is _incorrect_ not because it produces wrong results. On the contrary, it it produces precisely the expected results. I failed to read the exercise carefully:

> Refactor to remove all arguments by partially applying the functions.

“Functions”, in the plural. I partially applied only `filter`, but did not partially apply `match`, and the unit test caught that mistake!


### Solution 2 (correct)
```js
const filterQs = filter(match(/q/i));
```

So, we partially apply `filter`, passing it the predicate function. A new function is produced and assigned to `filterQs` which now expects the data to filtered through.

The predicate function itself (`match` in this case) is partially applied; we pass it the pattern, and it produces a function which expects the data to match against.

In both cases, we partially apply the function, which causes a new function to be produced. This new function remembers the previously passed argument and expects the remaining argument(s).

### Examples
```js
filterQs([]);
// []

filterQs(['quick', 'camels', 'quarry', 'over', 'quails']);
// [ 'quick', 'quarry', 'quails' ]
```

## Exercise C, max

Considering the following function:

```js
const keepHighest = (x, y) => (x >= y ? x : y);
```

Refactor `max` to not reference any arguments using the helper function `keepHighest`.

```js
// max :: [Number] -> Number
const max = xs => reduce((acc, x) => (x >= acc ? x : acc), -Infinity, xs);
```

### Solution

`keepHighest` is the same as the function created “inline” for `reduce` in the original version, just with different parameter names. Since `reduce` is curried, we just pass the `keepHighest` reference and `-Infinity` which causes a new function to be returned and assigned to `max`, which is now a function that remembers the first two parameters and expects the remaining argument, the `xs`'s.

```js
// max :: [Number] -> Number
const max = reduce(keepHighest, -Infinity);
```

### Examples

```js
max([10, -5, 15, 21, -103]);
// 21

max([]);
// -Infinity
```
