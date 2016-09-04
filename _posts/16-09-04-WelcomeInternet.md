---
title: Welcome to the Internet
excerpt: "In this blog post we develop our first Website"
teaser: internet.jpg
header:
  overlay_image: internet.jpg
  overlay_filter: 0.5
---
# Introduction


"Alice started to her feet, for it flashed across her mind that she had never before seen a rabbit with either a waistcoat-pocket, or a watch to take out of it, and burning with curiosity, she ran across the field after it, and fortunately was just in time to see it pop down a large rabbit-hole under the hedge.


In another moment down went Alice after it, never once considering how in the world she was to get out again.

The rabbit-hole went straight on like a tunnel for some way, and then dipped suddenly down, so suddenly that Alice had not a moment to think about stopping herself before she found herself falling down a very deep well." - Alice's Adventures in Wonderland, Lewis Carroll

## Welcome to the Internet...

Much like Alice in that wonderful tale, we sit at the entrance to a very deep tunnel. The internet is very much weirder than Carroll could ever have imagined. The sort of place where if you don't keep your feet, there's no knowing where you'll end up.

So today, let's take a baby step and look at the Week Three Maker's Academy challenge. The [Challenge Repository](https://github.com/makersacademy/rps-challenge) asks us to develop a website that allows us to play Rock Paper Scissors against a computer. We're given two user stories, as well as two bonus challenges:

```
As a marketeer
So that I can see my name in lights
I would like to register my name before playing an online game

As a marketeer
So that I can enjoy myself away from the daily grind
I would like to be able to play rock/paper/scissors

Bonus level 1: Multiplayer

Change the game so that two marketeers can play against each other ( yes there are two of them ).

Bonus level 2: Rock, Paper, Scissors, Spock, Lizard

Use the special rules ( you can find them here http://en.wikipedia.org/wiki/Rock-paper-scissors-lizard-Spock )
```

As with the airport challenge let's use TDD to build this website. But first we'll need to take a quick detour to look at Capybara and Sinatra.

## Zoology for Beginners - Capybara for tests.

So hopefully you've come across RSpec before  

## Fly Me To The Moon - An introduction to Sinatra

As much as I revere the man himself, unfortunately we'll have to explore his musical genius another time. For now we're going to focus on the Sinatra framework.

Sinatra, is a simple ruby based framework for building websites. It provides a structure for interfacing with the HTTP requests we get when we visit a website, allowing us to serve dynamic content.

Building with Sinatra is fairly straight forward. We require the gem in our gem file:

```
source 'https://rubygems.org'

ruby '2.2.3'

gem 'sinatra'
```
And then run `bundle`. That's about it. We're now ready to start building.

## Route 66 - Starting from the top
