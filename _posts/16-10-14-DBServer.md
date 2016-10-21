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
});
```

Next we need somewhere to save the data. We could keep the data in our controller, but as always we want to seperate our business logic from other areas of concern. To achieve this we'll create a lib file, and make a module to save our data.

As always, this means we'll need another set of tests. These ones are written in straight Jasmine as we're testing server side functionality. We'll check our storage is empty by default and then takes a query string and saves it to the state.

The `addToState` function itself takes a callback which is why we're making our assertions inside the method call. The function(error, success) pattern is very common in JS and we'll make use of it to check the data is saved properly.

```javascript
var DataStorage = require("../lib/data_storage.js")

describe("Data Storage Module", function() {

  beforeEach(function(){
    dataStorage = new DataStorage();
  });

  it("is initialized with an empty state", function() {
    expect(dataStorage._state).toEqual({});
  });

  it("can add items to state", function(next) {
    dataStorage.addToState("name=tom", function(err, success){
      expect(success).toEqual('{"name":"tom"}');
      next();
    });
  });
});
```

Now both feature and unit tests are in place, we can actually start writing code. Our `addToState` function will take in the query string, so we'll need to parse it into an object. We then combine this object into out state object, and return the result via the callback.

```javascript
function DataStorage (){
  this._state = {}
}

DataStorage.prototype =  {
  addToState: function(string, callback) {
    var obj = this._parseURI(string)
    Object.assign(this._state, obj);
    callback(null, JSON.stringify(obj))
  },

  _parseURI: function(str){
    var stringMatch = (str || "").match(/(\w+)=(\w+)/);
    var results = {}
    results[stringMatch[1]] = stringMatch[2]
    return results;
  }
};

module.exports = DataStorage;
```

This initial iteration doesn't deal with bad inputs, but lets put that on the back burner for another time. With the storage module written, we can then hook it up to our controller.

```javascript
var http = require('http');
var url = require('url');
var DataStorage = require('./lib/data_storage.js')
var dataStorage = new DataStorage

this.server = http.createServer(function (req, res) {
  var parsedURL = url.parse(req.url);

  var send_set_response = function(err, success){
      res.writeHead(201, {'Content-Type': 'JSON'});
      res.end(success);
  };

  if (parsedURL.pathname == "/set" && req.method == 'GET') {
    dataStorage.addToState(parsedURL.query, send_set_response)
  }
  else {
    res.writeHead(404, {'Content-Type': 'text/css'});
    res.end("Resource Not Found");
  }
});

exports.listen = function () {
  this.server.listen.apply(this.server, arguments);
};

exports.close = function (callback) {
  this.server.close(callback);
};
```

We've made a few small changes from the previous version of the controller. For one we've exported the listen and close functions which is what allowed us to use these functions in our feature test.

We're also using the url package to extract the query string from the url. Then we check if the path is /set and then tell our dataStorage to `addToState`, providing it with a callback.

At this point our tests should pass. We can add an item into our local memory.

## Step Three - Retrieving Keys

Alright, onto the next one. We want to be able to retrieve keys on a /get request. Again lets work from the outside in; Feature Test then Unit Test:

```javascript
// Feature Test:
describe("retrieving_key_from_object", function() {

  beforeEach(function(next){
    server.listen(5000);
    request(url + '/set?name=tom', function(err, res, body){next()});
  });

  afterEach(function(){
    server.close();
  });

  it("should successfully return params data on /set path", function(next) {
    request(url + '/get?key=name', function(error, response, body){
      expect(response.statusCode).toEqual(200);
      expect(body).toEqual('{"requested_value":"tom"}');
      next();
    });
  });
});

// Unit Test:
describe("Data Storage Module", function() {
  it("can retrieve items from state", function(next) {
    dataStorage.addToState("name=tom", function(err, success){})
    dataStorage.retrieveValue("key=name", function(err, success){
      expect(success).toEqual('{"requested_value":"tom"}');
      next();
    })
  });
});
```

Because our design is flexible and the inherit symmetry of the problem, we simply have to add the method to our lib file, then make the needed changes to our controller to add the route.

```javascript
DataStorage.prototype =  {
  // addToState: function(string, callback) {
  //   var obj = this._parseURI(string)
  //   Object.assign(this._state, obj);
  //   callback(null, JSON.stringify(obj))
  // },

  retrieveValue: function(string, callback) {
    var key = (this._parseURI(string) || {})['key']
    callback(null, JSON.stringify({requested_value: this._state[key]}))
  },

  // _parseURI: function(str){
  //   var stringMatch = (str || "").match(/(\w+)=(\w+)/);
  //   var results = {}
  //   results[stringMatch[1]] = stringMatch[2]
  //   return results;
  // }
};
```
Again this doesn't check that the key exists, which is something we'd want to rectify moving forward, but for our MVP it'll do. As for the controller we just add a GET /get route:

```javascript
this.server = http.createServer(function (req, res) {
  var parsedURL = url.parse(req.url);

  var send_set_response = function(err, success){
    res.writeHead(201, {'Content-Type': 'JSON'});
    res.end(success);
  };

  var send_get_response = function (err, success) {
    res.writeHead(200, {'Content-Type': 'text/css'});
    res.end(success);
  }

  if (parsedURL.pathname == "/set" && req.method == 'GET') {
    dataStorage.addToState(parsedURL.query, send_set_response)
  }
  else if (parsedURL.pathname == "/get" && req.method == 'GET') {
    dataStorage.retrieveValue(parsedURL.query, send_get_response)
  }
  else {
    res.writeHead(404, {'Content-Type': 'text/css'});
    res.end("Resource Not Found");
  }
});
```

And with this our tests should pass again. This is the most basic implementation of this server, but it meets the requirements of the brief and can be easily extended. 

## Final Thoughts

Clearly we need to defend against edge cases. Feel free to check out my [repo](https://github.com/TomStuart92/TechTests), though it's a useful exercise in it's own right.

In terms of design, our separation of business concerns has allowed us to easily add a database to our system. We'll simply need to change the way we add and retrieve from state in our lib folders. We won't have to change our controller whatsoever.

Overall, the code is fairly clean though could benefit from a further refactor. The request package is really simple and made testing super easy so it's one I'd recommend if you don't need the full functionality of a headless browser.
