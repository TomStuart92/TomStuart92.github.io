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
Let's quickly add a contains matcher, to check if one object contains the other. We'll use the `indexOf` method, which returns -1 if the object does not contain the search string. `indexOf` is a nice method to use as it is defined for both strings and arrays. Remember these indexes start at 0:

```javascript
[1,2,3,4].indexOf(2) // 1
'Hello Reader'.indexOf('l') //2
```
So our matcher becomes:

```javascript
function contains(container, target) {
  if (container.indexOf(target) === -1) {
    return('FAIL - '+ container + " doesn't include " + target);
  }
  else {
    return('PASS - '+ container + " includes " + target);
  }
}
```

These two matchers represent the vast majority of the immediate functionality we'll need. Using this template we can add more as needed, but lets move on to the remainder of the framework.

## Before It Describe

Lets turn to some of the other elements of our testing framework. We need It and Describe blocks to give some structure and content to our code, but these will do little more than invoke the code block we give them:

```javascript
function describe(description, itBlock) {
  console.log("DESCRIPTION: " + description);
  itBlock();
}

function it(itDescription, test) {
  console.log('   IT: ' + itDescription);
  test();
}
```
Before blocks are slightly more difficult. We want to invoke the before block before we run each it block.

Lets allow the user to define a before block in there code and then just call it when we run the it block. We'll need to account for the chance the user has not defined a before block.

```javascript
function it(itDescription, test) {
  console.log('   IT: ' + itDescription);
  (beforeEach || Function)();
  test();
}
```
Here we use the || function to return either the beforeEach if defined, or otherwise the blank function object which will have no effect if called.

With these pieces in place, we can write a suite of simple tests:

```javascript
describe("Note", function(){

  beforeEach(function(){
    note = new Note("Practice Note", "This is a practice note, it's not very interesting");
    return "done!";
  });

  it("should have a title", function(){
    expect(equal(note.title, "Practice Note"));
  });

  it("should have content", function() {
    expect(equal(note.content, "This is a practice note, it's not very interesting"));
  });

  it("should preview first 20 characters", function() {
    expect(equal(note.preview(), "This is a practice n"));
  });

});
```

We can build other elements on top of our framework as needed, but with these elements we can test most simple objects. We can also use Javascript's built in DOM-manipulation methods to allow us to feature test our websites!
