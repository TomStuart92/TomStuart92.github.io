---
title: Back to the tenth grade
excerpt: "It's time to build a testing framework"
teaser: test.jpg
header:
  overlay_image: test.jpg
  overlay_filter: 0.5
---

# Introduction

This week we've been tasked with building a single page application entirely in Javascript. This means no other frameworks so no Jasmine, Node, Express, or jQuery to help out. The first challenge we had to solve is that without a testing framework, it becomes pretty difficult to test drive your applications development.

So to fix this, our first task was to build our own custom test framework to allow us to TDD the application.

## Snap - It's a Match.

Let's start with some matchers. The core functionality of a testing framework comes from being able to assert one thing is the same as or different from another.

So lets start with a simple function that checks if the two arguments given to it are equal, and returns a nice PASS/FAIL message.

```javascript
function equal(a,b) {
  if (a.toString() === b.toString()) {
    return('PASS - '+ a + " equals " + b);
  }
  else {
    return('FAIL - '+ a + " doesn't equal " + b);
  }
}
```

There's an interesting distinction to be made in javascript between the `==` and `===` operator.

- `==` is an abstract equality. It will attempt to resolve different data types if different.
- `===` is a strict equality operator. The objects must be of the same type to pass.

```javascript
console.log('5' == 5) // true
console.log('5' == '5') // true

console.log('5' === 5) // false
console.log('5' === '5') // true
```

In our matcher we have forced the coercion to be to a string, and then checked the strict equality. The abstract equality follows different rules for how it attempts to resolve data types, which can lead to difficulties.

The reason we need to coerce at all is for arrays. In javascript, two arrays are not the same, even if they have the same content:

```javascript
console.log([1,2,3,4] === [1,2,3,4]) //false
console.log([1,2,3,4].toString() === [1,2,3,4].toString()) //true
```
