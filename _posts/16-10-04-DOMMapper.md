---
title: Mapping Interactions
excerpt: "Building a Ruby Domain Mapper"
teaser: map.jpg
header:
  overlay_image: map.jpg
  overlay_filter: 0.5
---

# Introduction

Next week at Makers we have our first project week. This is an opportunity to build something we choose, in a stack of our own choosing. The theme for the week is Dev Tools, that is, any tool that makes our lives as developers easier.

I had an idea for such a tool, but being me, I wanted to prove it was feasible before offering it up. So I played around with some code, and a few hours later had a working prototype.

This blog post will look at the code for that prototype. It's important to remember that this code is not tested and follows none of the key design principles. Really it's awful code in all but the most important aspect; it works!

# The Idea

Regular readers will remember that on a few occasions, I have shown you domain models of code I was working on. If you think back to the [airport challenge post]({{ site.url }}/Airports), we had a domain model that looked a little like this:

![My helpful screenshot]({{ site.url }}/images/airport_domain.jpeg)
