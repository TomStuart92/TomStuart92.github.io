---
title: It's time for Final Projects
excerpt: "Recollections from my Final Project"
teaser: swift.jpg
header:
  overlay_image: swift.jpg
  overlay_filter: 0.5
---
## Week Eleven

Week Eleven has finally arrived, and that means it's time for final projects. For the next two weeks we'll be working on a group project which will test everything we've learnt over the last ten weeks.

I'm going to keep a day to day diary of these two weeks to chart the highs and lows of a development sprint. Hopefully more highs than lows, but who knows!

### Monday 24th October

We learnt about our groups and projects today. I'm very happy with my team, we've got a great group of people and everyone brings a great set of skills to the table.

As for the project. It seems life has a sense of humour, We're building a dating application. The idea is to build on the wild success of Tinder, but bolt a layer on the front. A user will be shown a list of events and then select events that interest them. They'll then only be shown people who have also liked the same events, the idea being they already have a common interest to talk about and first date event to attend.

It's actually a really nice project in terms of showing off everything we've learned. For one we're going to bite the bullet and build it as a mobile application. This means a new language/framework/paradigm. But it also is heavy on business logic, database querying and dynamic front-ends.

We spent the day deciding on what language to use for the mobile-app. We have a choice of Swift(IOS), Java(Android) or React Native(Both). Given the nature of our application, I was in favour of React Native, but the group has settled on Swift, as it apparently has a shallower learning curve. It's also probably a better CV language to which is good.

We also think we'll use firebase as our cloud database to keep things simple. I'm about 50/50 as to whether this will service our needs. I guess we'll find out soon enough.

### Tuesday 25th October

After some serious pre-sleep thinking and a big group discussion, we've decided that firebase isn't going to be suitable for our needs. Lot's of our business logic needs to sit above the database, and I don't want to push that to the app side.

This means that as well as building our IOS app, were also going to build our own cloud server to service the apps. In two weeks. Well I do like a challenge. The project is beginning to take some form. A good diagram is always the best way to understand whats going on, so here's a lifecycle diagram of our app:

![My helpful screenshot]({{ site.url }}/images/attendr_diagram.png)
