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

<svg id="chart" width="800" height="500" xmlns="http://www.w3.org/2000/svg">
  <!-- horrible region -->
  <path d="M50 450 L 50 0 L 800 0 L 800 450 Z" fill="#ff8989"></path>
  <!-- bad region -->
  <path d="M50 450 L 800 0 L 800 450 Z" fill="#FFC543"></path>
  <!-- fair region -->
  <path d="M50 450 L 800 450 L 800 330 Z" fill="yellow"></path>
  <!-- good region -->
  <path d="M50 450 L 800 450 L 800 410 Z" fill="#C8EA00"></path>
  <!-- excellent region -->
  <path d="M50 450 L 800 450 L 800 440 Z" fill="#53d000"></path>

  <!-- axes -->
  <path d="M50 0 L 50 450 L 800 450" fill="transparent" stroke="black" stroke-width="2"></path>

  <path d="M50 448 L 800 448" fill="transparent" stroke="black" stroke-width="2"></path>
  <text x="700" y="438" fill="black">O(log n), O(1)</text>

  <path d="M50 450 L 800 400" fill="transparent" stroke="black" stroke-width="2"></path>
  <text x="760" y="390" fill="black">O(n)</text>

  <path d="M50 450 Q 400 350, 800 150" fill="transparent" stroke="black" stroke-width="2"></path>
  <text x="630" y="190" fill="black">O(n log n)</text>

  <path d="M50 450 Q 180 380, 250 0" fill="transparent" stroke="black" stroke-width="2"></path>
  <text x="260" y="30" fill="black">O(n^2)</text>

  <path d="M50 450 C 100 430, 120 350, 120 0" fill="transparent" stroke="black" stroke-width="2"></path>
  <text x="125" y="20" fill="black">O(2^n)</text>

  <path d="M50 450 C 80 450, 80 350, 80 0" fill="transparent" stroke="black" stroke-width="2"></path>
  <text x="80" y="20" fill="black">O(n!)</text>

  <text x="0" y="0" transform="translate(30 230) rotate(-90)" style="dominant-baseline: middle; text-anchor: middle; font-size:20px; color: #555; font-size:20px; color: #555; font-style: italic;" fill="black">Operations</text>
  <text x="0" y="0" transform="translate(420 470)" style="dominant-baseline: middle; text-anchor: middle; font-size:20px; color: #555; font-style: italic;" fill="black">Elements</text>
</svg>

Credit - [Big O Cheatsheet](bigocheatsheet.com) 
