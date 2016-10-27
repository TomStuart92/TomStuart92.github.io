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

Today we were down to four people, so we divided up. Two of us started looking at the swift app. Euan and I, turned our attention to the cloud server. We've settled on Node as our server side framework. We wanted something that we were familiar with, but also that could deal with a (hypothetical) large number of connections.

Our first task was to get a list of events into a database to show to users. We've decided to use MeetUps api to get events in London for our MVP. So we wrote a script that makes a request to their API and saves the response to a Postgres Database. We then deployed this to Heroku, and set up an automated task, so our events are refreshed on a regular basis.

All in all a good day, but I feel like we haven't made as much progress as we should have. Swift/XCode appears to be a really difficult environment to work in, and the development cycle may prove to be much longer than we originally thought.

### Wednesday 26th October

I felt really tired/frustrated/unmotivated today until about 4pm. This morning I quickly built the route to serve up the events in the database. [It's here in case anyone is interested](http://www.attendr-server.herokuapp.com/events).

The rest of the day was spent in two teams, one trying to pull this data into swift, the other trying to implement Facebook logins. This was generally met by frustration. One of the issues we're coming up against is the recent release of Swift3. This means almost all the tutorials and documentation online is now out of date.

Still at 4pm we had simultaneous breakthroughs as both teams achieved their goals. This was a great feeling and has really energised the team. Definitely been a trying day, but I think we're slowly getting the hang of Swift and I think tomorrow, we should be able to make some good progress.
