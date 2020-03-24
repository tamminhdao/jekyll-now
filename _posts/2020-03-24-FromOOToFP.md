---
layout: page
title: From OO to FP Paradigm Shift - Immutability (need a better name)
---

The problem:

Given a phrase, count the occurrences of each word in that phrase. 
For simplicity, let's assume that our phases only include lowercase letters and no number or special characters.
For the input "blue cat black cat blue bird", we expect the result in the form of key-value pairs,
like so `{ blue: 2, cat: 2, black: 1, bird: 1 }`


In the land of object oriented programming, it is a common pattern to assign a placeholder variable 
and use a loop to update that variable until we get the result.

Here are the steps to solve the word count problem:

1. Convert the input string into an array of strings.
2. Assign a variable `count` as an empty placeholder for our final result.
3. Use to a loop to iterate through the array of strings and update `count` with each iteration.
4. Return the result.

```js
export const countWords = phrase => {
  const words = phrase.split(" ");
  const count = {}; 
  for (let i = 0; i < words.length; i++) {
    count[words[i]] = count[words[i]] ? ++count[words[i]] : 1;
    console.log(count);
  }
  return count;
};
```

If we add a `console.log` in the for loop to peak at our variable `count`, we can see it changes with each iteration.
```bash
  console.log word-count.js:6
    { blue: 1 }

  console.log word-count.js:6
    { blue: 1, cat: 1 }

  console.log word-count.js:6
    { blue: 1, cat: 1, black: 1 }

  console.log word-count.js:6
    { blue: 1, cat: 2, black: 1 }

  console.log word-count.js:6
    { blue: 2, cat: 2, black: 1 }

  console.log word-count.js:6
    { blue: 2, cat: 2, black: 1, bird: 1 }
```

How would the solution to this problem look like in functional programming?

There are a lot of aspects that differentiate functional from object oriented programing. Let us zero in on just one for now: Immutability - an object cannot be modified after it's created. With this restriction in place, our prior solution is no longer valid since it relied on being able to mutate data freely. But don't worry!

Although we've lost our trusted for and while loops, we have also gained a few new powerful friends - `map`, `filter` and `reduce`. These functions are the stable of functional languages. They have to be called on arrays in Javascript but instead of mutating the original array, they create and return a new one. This is how we satisfy the immutability requirement. Let's go through each of these functions and see how they work.

## `Array.map()`

Given an array of strings `["snake", "cat", "cat", "bird"]`.

Iterate through this array returning 1 whenever we meet a `"cat"` and 0 otherwise.

```js
const pets = ["snake", "cat", "cat", "bird"]
const result = pets.map(pet => pet === "cat" ? 1 : 0)

console.log(result) // [0, 1, 1, 0]
console.log(pets) //["snake", "cat", "cat", "bird"]
```
`Array.map()` applies the anonymous function `pet => pet === "cat" ? 1 : 0` on every elements of the `pets` array and return 
a new array `result`. 

It is a one to one relationship in the sense that an input array of n elements will output a result array with the same number of elements in the corresponding order.
We can confirm that our original array `pets` was not altered in the process by `console.log` it at the end.


## `Array.filter()`
Given the an arrays of strings `["blue", "cat", "black", "cat", "blue", "bird"]`.

How would you eliminate duplication and return an array of unique strings `["blue", "cat", "black", "bird"]`?

```js
const allWords = ["blue", "cat", "black", "cat", "blue", "bird"];
const filteredResult = allWords.filter(
  (value, index, array) => array.indexOf(value) === index
);

console.log(filteredResult); // ["blue", "cat", "black", "bird"]
console.log(allWords); // ["blue", "cat", "black", "cat", "blue", "bird"]
```

`Array.filter()` applies the anonymous function `(value, index, array) => array.indexOf(value) === index` on every elements of `allWords` array and
returns a new array `filteredResult` with only the elements where that anonymous function returns true.

Again, we `console.log` at the end to double check that our original array `allWords` did not change.


## `Array.reduce()`
The reduce function takes a function (commonly refered to as the reducer) and an initial value.
The reducer function takes 4 arguments, two of them are optional depending on the need of your transformation.


A popular demonstration of reduce with a reducer function of 2 arguments (the accumulator and the current value) is to reduce a array of integer to its sum.

```js
const sum = [1, 2, 3].reduce((accumulator, current) => {
  console.log(`Accumulator: ${accumulator}`)
  return accumulator + current
}, 5)

console.log(`Sum: ${sum}`) // 5 + 1 + 2 + 3 = 11
```

By logging out the value of the accumulator, we can see how it starts from the provided initial value of 5, 
adding the current value in each iteration, and finally arrives at the sum of 11.

```bash
  console.log reduce.js:2
    "Accumulator: 5"

  console.log reduce.js:2
    "Accumulator: 6"

  console.log reduce.js:2
    "Accumulator: 8"

  console.log reduce.js:4
    "Sum: 11"
```

Here is an example where the reducer needs that optional 3rd argument (the index).

Given 2 arrays of the same length `["dog", "cat", "bird"]` and `[3, 3, 2]`.

With all the items of the first array as keys in an object, map all the items of the second array as their values, in corresponding order, like this:
`{ "dog": 3, "cat": 3, "bird": 2 }`

```js
const friends = ["dog", "cat", "bird"]
const age = [3, 3, 2]

const friendsAge = friends.reduce((accumulator, current, index) => {
  accumulator[current] = age[index]
  return accumulator
}, {})

console.log(friendsAge) // { "dog": 3, "cat": 3, "bird": 2 }
console.log(friends) // ["dog", "cat", "bird"]
console.log(age) // [3, 3, 2]
```

Now that we have armed ourselves with these new tools, let's jump back on the word count problem and solve it without mutating a single value.
Here is a hint: this problem can be solved with one reduce function.

If you don't see it yet, no worries. It can take a while to understand `reduce` and then some more to master the instinct of knowing when to grab for it (speaking from personal experience).
Moreover, there are multiple ways to solve this problem. A oneliner reduce function is not the only solution.

Here is another hint: Look at all the examples we just covered to understand `map`, `filter` and `reduce`. 
Can you use any of them to fomulate a solution? Can you use all of them? (surprise! surprise!)

Let's walk through a solution to this problem. It's not the shortest solution. But it will be easy to understand because you will soon realize that we have covered every part of it already.
Once again, for the input phrase `"blue cat black cat blue bird"`, we expect the result  `{ blue: 2, cat: 2, black: 1, bird: 1 }` 

Here are the steps:

Step 1. Convert the input string into an array of strings.
Conveniently, `Array.split()` gives us back an array
`["blue", "cat", "black", "cat", "blue", "bird"]` to operate on.

Step 2. Filter out any duplication and keep only the unique strings
`["blue", "cat", "black", "bird"]`.
Didn't we just do exactly that with `Array.filter()` a few minutes ago?

Let's pause and think for a moment.
While learning about `Array.reduce()`, we demonstrated that we can merge two array into an object with corresponding key value pairs.
We got one array from step 2. If only we can have an array of the corresponding counts so we can reduce them into our result.
That's exactly what we are going to do next.

Step 3. The goal here is to produce an array with the count of each unique string in the phrase.
This can be further broken down into a few smaller steps.

  3a. Our input is the array `["blue", "cat", "black", "bird"]` and our desire output is `[2, 2, 1, 1]`,
  that is a sign that we should call `map()` on the input with a function that does the counting for us.

  3b. Write the function we need to pass into `map()`.

  Hint: Look at the example we did for `Array.map()` and the sum in `Array.reduce()`.
  You can also chain them like this `myArray.map(() => {}).reduce(() => {})`.
  
Step 4. Now that we have an array of unique strings and an array of counts. 
Use `Array.reduce()` to reduce them into one result. 
This is exactly what we demonstrated above with a reducer function of 3 arguments. 


Here is the final solution:

```js
const uniqueWords = (allWords) => {
  return (
    allWords
    .filter((value, index, array) => 
      array.indexOf(value) === index)
  )
}

// 3b
const count = (target, words) => {
  return( 
    words
    .map(item => item === target ? 1 : 0)
    .reduce((acc, curr) => {
      return acc + curr
    }, 0)
  )
}

export const countWords = (sentence) => {
  const all = sentence.split(" ")
  const unique = uniqueWords(all)
  const uniqueCount = unique.map(word => count(word, all)) // 3a

  // 4
  return unique.reduce((acc, curr, index) => {
    acc[curr] = uniqueCount[index]
    return acc
  }, {})
};

```

As for the oneliner solution, I will not put it here in case you want to figure it out on your own.
