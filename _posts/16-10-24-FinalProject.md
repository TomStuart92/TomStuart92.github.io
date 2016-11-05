---
title: It's time for Final Projects
excerpt: "Recollections from my Final Project"
teaser: swift.jpg
header:
  overlay_image: swift.jpg
  overlay_filter: 0.5
---
## Final Projects are Here!

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

I felt really tired/frustrated/unmotivated today until about 4pm. This morning I quickly built the route to serve up the events in the database. [It's here in case anyone is interested](https://attendr-server.herokuapp.com/events).

The rest of the day was spent in two teams, one trying to pull this data into swift, the other trying to implement Facebook logins. This was generally met by frustration. One of the issues we're coming up against is the recent release of Swift3. This means almost all the tutorials and documentation online is now out of date.

Still at 4pm we had simultaneous breakthroughs as both teams achieved their goals. This was a great feeling and has really energised the team. Definitely been a trying day, but I think we're slowly getting the hang of Swift and I think tomorrow, we should be able to make some good progress.

### Thursday 27th October

My laptop is dying. It's taking progressively longer to run even the most simple tasks. And don't even think about trying to run a simulation in xCode. This presents a pretty challenging set of circumstances for developing an application in Swift.

Luckily today I was able to focus on our back-end. Given this is written in Node, I can work on this in Atom without much difficulty.

One of the most important things in any project is context. It's no different in programming. The context of this project is that we have two weeks to develop a working prototype. It certainly doesn't make sense spending months develop some beautiful controller logic, if your client needs a prototype by the end of the week.

The implications of this guide where we focus the majority of our time. Its already clear the majority of the technical challenges lie in the IOS app, and so the back-end server is less important. Given that people don't remember your project for the quality of your code, but on the visual features, this will be were we will spend most our time.

So today I worked alone, and smashed out the majority of the needed back-end functionality. It's certainly not the prettiest code but it's fully tested and working. For our MVP that's all it needs to do.

Building the matching logic proved to be the hardest thing. The sequelize docs are pretty useless, so I ended up falling back on my Data Analytics Brain and writing the query in raw SQL.

```javascript
return models.sequelize.query(
      'SELECT * FROM "Users" WHERE id in'+
      '(SELECT "UserId" FROM "RSVPs" WHERE "UserId" <> '
      +UserId
      +' AND "EventId" in (SELECT "EventId" FROM "RSVPs" WHERE "UserId" = '
      +UserId
      +'))', { type: models.sequelize.QueryTypes.SELECT}
    ).then(function(results){
      return results})
  }
```

Isn't that beautiful? It's certainly a code smell, but again, at the moment I more worried about having a working final project than a beautiful code base. The technical debt can be paid of later.

### Friday 28th October

As a team, we continue to have regular little wins. This is a great feeling, and really makes it feel like your progressing towards your goal. Thao and I managed to connect our application up to our backend that I built yesterday. This was a great feeling and really validated our approach of building the two apart.

Elsewhere we've got a working User creation system, and our messaging system is progressing nicely. I think we're in a really good place to smash out the rest of the features before the feature freeze on Wednesday.

### Saturday 29th October

Yep, didn't do any coding today. Had an incredible truffle burger and great coffee though.

### Sunday 30th October

The advantage of remote work is that you can be curled up in front of a roaring fire, with Star Wars on the screen whilst you smash out a few outstanding issues.

I tidied up the Facebook login today, pushing Users through to events on a successful login and checking if they're already logged in when they arrive on the login screen. I'm starting to get into a flow when writing Swift, but it's still a bit of a challenge. Most of this seems to stem from the static typing, something I think I might devote a blog post to next week. It's not intrinsically difficult, but requires a shift in mindset from dynamic languages.

I also played around with some of the visual styling which is something that seems to play to xCode strengths. As it gives you an actual wireframe of each scene in the storyboard it becomes pretty easy to get things looking just as you want.

I'm starting to feel we're on track to complete our project to a really decent level by Friday. Hopefully, I'm not mistaken.

### Monday 31st October

So here we sit, at the end of all things. The last week of Makers Academy. There's a definite sense of impending doom across the cohort as the realisation of next weeks return to real life hits.

But, there's still project work to be getting on with. Today started with a code review of the work we've done over the weekend. There were some impressive visual changes made, and we're getting really close to our MVP.

The rest of the day was spent implementing matches and working on the chat functionality. The matches have been fully implemented now, complete with profile picture and the name of the event your both attending. The chat is proceeding a little slower, as we're using Firebase to power it. This is proving to be a configuration nightmare, but headway is being made.

As I left we discovered we've got a merge issue on our storyboard which will be really difficult to resolve, as it's a visual tool, not lines of code. Fun for tomorrow!

### Tuesday 1st November

MVP, MVP! Well today we achieved the inevitable and got our project to it's MVP. MVP stands for Minimal Viable Product, and is the first time your project can be considered a viable entity. Today we got the chat messaging working and as such can call our app a fully functioning dating application!

I was really impressed at just how easily we were able to integrate the chat functionality into the app. It's been another victory for building things separately and then merging them together. In this case we built the chat as a standalone application first, which we then brought in to our main app once it was working.  

Elsewhere, we've mostly been making the small changes that are needed to really polish the app off. I quickly added Map functionality for viewing the location of an event, we added some animation, and worked on the styling. It's all coming together really nicely and should be really polished for Friday.

### Wednesday 2nd November

Now that we've reached the MVP stage, the remainder of the week will be spent ironing out the bugs, cleaning the code and polishing the visuals. It's a chance to pay back some of the technical debt we accrued earlier in the project. There will always be little dirty hacks you can use to move faster in building a system, but at the appropriate moment you should always go back and clean them up.

The project itself is looking great and it's certainly something I'll be happy to show off in my portfolio. It's been a really interesting challenge technically and very representative of the kind of work we could be looking at in our junior developer jobs.

We also had our final careers sessions today, which involved a really useful group conversation about the fears we have about finding jobs. Open dialogue like this is always an enlightening exercise; as it turns out everyone has pretty similar feelings. They ranged from the expected - "I'm not good enough", to the downright bizarre - "I'm worried I'll use up to many company perks". Still fears like these are always best out in the open where you can examine them properly.

Personally, I'm not worried about finding a job per se, but do fret that choosing the wrong job will make a big impact on my career progression and long term sustainability of my development career. It's almost definitely unfounded, as I'm sure the first job has little bearing over a full 40 year career, but alas as usual our fears probably don't stand up to logical explanation.

### Thursday 3rd November

The end is nigh, or something like that. My penultimate day at Makers Academy passed with little aplomb. Our last issues were closed, pull requests merged and commits made this morning. Officially, Attendr V1 is complete. I'm really happy with just how polished a product we were able to create in two weeks using entirely self-taught technologies.

We spent the rest of the day putting together our presentation for graduation tomorrow evening. This involved recording a screencast of our app in use, which thanks to some creative back-end work, turned out to be suitably hilarious.

I'm going to speak around the tech side of the project tomorrow which should be fine. It's a really interesting approach and the breadth of our project should really show through.

Apart from a delicious Team Lunch, their is little else to speak on. I'm a good combination of happy, excited, sad, scared, and thankful as I head into my last day tomorrow. This has been one of the greatest experiences of my life, and I want to make sure it has a fitting conclusion.

### Friday 4th November

Return of the Jedi, Return of the King, The Deathly Hallows; all epic sagas must come to an end. So it is with out Makers Academy journeys. Today bore many tears, hugs and smiles. A day of tumultuous emotion as we reach the end of our time on this incredible course.

It was incredibly sad to say goodbye to the group of people I've spent the last 16 weeks with. The bond thats formed through mutual hardship is pretty incredible, and it's been an absolute pleasure getting to know each and every member of the Makers Academy family.

As for the day itself, we had our a careers fare this morning which was a great opportunity to speak to some real developers who are looking to hire junior developers. It was interesting to pick their brains about exactly what they're looking for.

The rest of the day was spent hanging out and putting the finishing touches to our presentation. [You can view our final video demo here](https://youtu.be/a6o0Nbv4VA8) At 5.30 the presentations started, and everyone absolutely killed it. All the presentations were excellent, and a fitting end to the course. Then a few beers and slices of pizza to end an emotionally exhausting day.

Words can't really express how I feel right now. It's almost an empty deflated feeling as I again have no structure in my life. For three months, Makers has provided me a bubble in which I could totally devote myself to coding. Now it's just me against the world. But, I feel totally prepared for what's to come.

So what next? There's still so much to learn, and of course the hunt for a job begins tomorrow. I also want to focus on picking up some of the more computer science side of things so I'll start looking at that as well. Otherwise, it's more coding, testing, and getting very annoyed at xCode.
