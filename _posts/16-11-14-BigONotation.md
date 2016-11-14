---
title: Big O Notation
excerpt: "Why is my program running so slow?"
teaser: bigo.jpg
header:
  overlay_image: bigo.jpg
  overlay_filter: 0.5
---

# Introduction

Learning to program in 12 weeks means you're always going to miss a few things. At Makers Academy, a lot of this content is what would more generally fall in the Computer Science area. Generally, these are things you don't need to know to write a computer program, but if you do you'll get a far deeper understanding of what your program is doing.

So, for the next few weeks I'm going to write a few posts about some of the more interesting things I feel are missed from the lightning strike that is the Makers Academy course.

Today, we're going to focus an answer to the question, 'why is my program running so slow?'. This is somethings we've all encountered before, and is a really useful thing to be able to bring up in a job interview or tech test.

# Big O Notation.

One of the main things that affects the speed at which your program runs is the size of the input you give it. An operation on array with two elements will generally be faster than an operation on an array with a million elements.

We use Big O notation to understand how programs scale with an increasing number of inputs. Now there is a whole load of maths that underpins this idea, and if you are so inclined it's really interesting. But it's not really necessary at least to get a high level understanding.

The main idea is that we identify a mathematical function that reflects how our program will handle increased input size. The main functions are below:

<svg id="chart" width="700" height="500" xmlns="http://www.w3.org/2000/svg">
  <!-- horrible region -->
  <path d="M50 450 L 50 0 L 700 0 L 700 450 Z" fill="#ff8989"></path>
  <!-- bad region -->
  <path d="M50 450 L 700 0 L 700 450 Z" fill="#FFC543"></path>
  <!-- fair region -->
  <path d="M50 450 L 700 450 L 700 330 Z" fill="yellow"></path>
  <!-- good region -->
  <path d="M50 450 L 700 450 L 700 410 Z" fill="#C8EA00"></path>
  <!-- excellent region -->
  <path d="M50 450 L 700 450 L 700 440 Z" fill="#53d000"></path>

  <!-- axes -->
  <path d="M50 0 L 50 450 L 700 450" fill="transparent" stroke="black" stroke-width="2"></path>

  <path d="M50 448 L 700 448" fill="transparent" stroke="black" stroke-width="2"></path>
  <text x="550" y="438" fill="black">O(log n), O(1)</text>

  <path d="M50 450 L 700 400" fill="transparent" stroke="black" stroke-width="2"></path>
  <text x="550" y="400" fill="black">O(n)</text>

  <path d="M50 450 Q 400 350, 700 150" fill="transparent" stroke="black" stroke-width="2"></path>
  <text x="550" y="190" fill="black">O(n log n)</text>

  <path d="M50 450 Q 180 380, 250 0" fill="transparent" stroke="black" stroke-width="2"></path>
  <text x="260" y="30" fill="black">O(n^2)</text>

  <path d="M50 450 C 100 430, 120 350, 120 0" fill="transparent" stroke="black" stroke-width="2"></path>
  <text x="125" y="40" fill="black">O(2^n)</text>

  <path d="M50 450 C 80 450, 80 350, 80 0" fill="transparent" stroke="black" stroke-width="2"></path>
  <text x="80" y="20" fill="black">O(n!)</text>

  <text x="0" y="0" transform="translate(30 230) rotate(-90)" style="dominant-baseline: middle; text-anchor: middle; font-size:20px; color: #555; font-size:20px; color: #555; font-style: italic;" fill="black">Complexity</text>
  <text x="0" y="0" transform="translate(420 470)" style="dominant-baseline: middle; text-anchor: middle; font-size:20px; color: #555; font-style: italic;" fill="black">Elements</text>
</svg>

Credit - [Big O Cheatsheet](bigocheatsheet.com)

As you can see, as we increase the size of our input, along the x axis, the time-complexity shown on the y-axis scales differently. Some programs change very little when we increase the number of elements, such as O(log n) programs. Others change a lot with very small increases  in input size such as O(n!) algorithms.

If we can identify code patterns that lead to these different outcomes, we can design our code so that it scales in a more linear fashion and so is less complex for large inputs.

The best way to understand is for an example, so below I'll give an example of code which leads to each of the major BigO complexities.

# O(1) - Constant Time

Code that is O(1) means that the time-complexity of the code doesn't change with increased input size. The example below shows an example of integer manipulation where the computer is only doing two operations. The difference in dividing 2/1 and dividing 10^10 / 2 is miniscule and so we can assume that this algorithm takes a constant amount of time.

```java
int mod(int a, int b) {
  if (b <= 0){
    return -1;
  }
  int div = a / b;
  return a - div * b;
}
```

# O(n) - Scales linearly with the size of the input

O(n) algorithms scale linearly. This means if we double the input size, we double the complexity of the operation.

Below we have a function that multiplies two numbers a and b together using a for loop. The computer has two add two numbers together b times. This means our algorithm will scale as O(b).

```java
int product(int a, int b) {
  int sum = 0;
  for (int i = 0; i < b; i++) {
    sum += a;
  }
  return sum
}
```

# O(log n)

Lets look at something a little harder. The code below is a recursive algorithm to find the square root of a number. It divides the range 0 to n into two halves, then checks if the halfway point is the square root. If not, it looks at whether the half point is too large or too small. It then repeats the search in the correct half.

This means that on each iteration the size of the data set gets cut in half. These sort of searches form a family called binary searches. Any algorithm where the data set is cut in half iteratively is known as a O(log n) function.

```java
int sqrt(int n){
  return sqrt_helper(n, 1, n);
}

int sqrt_helper(int n, int min, int max){
  if (max < min) return -1;

  int guess = (min + max) / 2;
  if (guess * guess == n){
    return guess;
  } else if (guess * guess < n) {
    return sqrt_helper(n, guess+1, max);
  } else {
    return sqrt_helper(n, min, guess -1);
  }
}
```

# O(n log n)

A function is O(n log n), if like above it cuts the search set in half on each iteration, but it does something like a for loop in each iteration that also scales with n. For a contrived example:

```java
int do_something(int n, int min, int max){
  if (max < min) return -1;

  int sum = 0;
  for (int i = 0; i < n; i++) {
    sum += i;
  }

  int guess = (min + max) / 2;
  if (guess * guess == n){
    return guess;
  } else if (guess * guess < n) {
    return do_something(n, guess+1, max);
  } else {
    return do_something(n, min, guess -1);
  }
}
```

# O(n^2)

Below we have a double nested for loop. This means for each value a, we have to do the operation for each value of b. Therefore, the function is O(a*b) which in the case that a == b == n, is the same as O(n^2). These functions scale very quickly with increasing n and should be avoided!

```java
int function(int a, int b) {
  for (int i = 0; i < a; i++) {
    for (int i = 0; i < b; i++) {
      // do something
    }
  }
}
```

It's really worth exploring the maths behind this further if you want to truly understand why your triple nested for loop is running so slowly. It's something that will help you become a better developer.
