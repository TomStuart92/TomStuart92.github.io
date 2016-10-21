---
title: Tech Testing Today
excerpt: "Building a Database Server"
teaser: db_server.jpg
header:
  overlay_image: db_server.jpg
  overlay_filter: 0.5
---

## Introduction

This week at Makers Academy, we've got a whole week devoted to tech tests. Everyday we're being set a new challenge which we've got the rest of the day to code in whatever language we want. At the end of the day, we regather to review each others code.

[I've got a single repository for the weeks work, which you're more than welcome to have a look through (potential employers doubly so).](https://github.com/TomStuart92/TechTests)

The challenge for Day One was to build a database server. The brief in full was to:

```
Before your interview, write a program that runs a server that is accessible on http://localhost:4000/.
When your server receives a request on http://localhost:4000/set?somekey=somevalue it should store the passed key and value in memory.
When it receives a request on http://localhost:4000/get?key=somekey it should return the value stored at somekey.
During your [mock] interview, you will pair on saving the data to a file.
```

We can instantly pull a few things out of this:
- We're going to need a server, with at least two routes (/set and /get).   
- The presence of query strings on these routes tell us they need to accept GET requests.   
- We'll need some way to persist the given params in local memory.   
- Our system needs to be easily extendable to add a database.   

## Step One - Choosing Tech and Setup

This challenge screams lightweight implementation. We don't need the associated baggage of a framework like Rails. All we want is a slimmed down server, and some associated logic.

I decided to use Node to power my solution. Without express it's a really lightweight framework, that strips away everything but the essentials we need for our project.

For testing I settled on Jasmine combined with a node package called request which allows us to send HTTP requests without the added complication of a headless browser.

Setting up a node project is as simple as running `npm init` then installing our packages using `npm install --save #packageName`.

To get a simple server up and running we use the http package:

```javascript
var http = require('http');

http.createServer(function (req, res) {
  res.writeHead(200, {'Content-Type': 'text/css'});
  res.end("Hello World!");
}).listen(4000);
```

This server just displays Hello World on any path. Hardly very useful!

## Step Two - Saving Keys

Alright, let's get building. We're going to first build a route that allows us to save a key into local memory. To begin lets write a feature test for our new route.

We're using Jasmine, which regular readers will be familiar with. Usually for feature testing we'd use a headless browser so that we can click on buttons and links on our page.

However for this, we're simply making http requests to a server. Therefore, we'll use the request package which gives us the ability to make requests from our tests.

We import our sever package so that we can run a test version of our server. Before each test we create a new instance of it, and then close if after the test.

Our feature test makes a request to http://localhost:5000/set?name=tom, and asserts we get the correct response code and body.

```javascript
var server = require('../../index.js');
var request = require('request');
var url = "http://localhost:5000";

describe("saving_key_to_object", function() {

  beforeEach(function(){
    server.listen(5000);
  });

  afterEach(function(){
    server.close();
  });

  it("should successfully return params data on /set path", function(next) {
    request(url + '/set?name=tom', function(error, response, body){
      expect(response.statusCode).toEqual(201);
      expect(body).toEqual('{"name":"tom"}');
      next();
    })
  });
```

Next we

## Step Three - Retrieving Keys

## Design Thoughts
