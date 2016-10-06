---
title: Mapping Interactions
excerpt: "Building a Ruby Domain Mapper"
teaser: map.jpg
header:
  overlay_image: map.jpg
  overlay_filter: 0.5
---

# Introduction

Next week at Makers we have our first project week. This is an opportunity to build something we choose, in a stack of our own choosing. The theme for the week is Dev Tools, any tool that makes our lives as developers easier.

I had an idea for such a tool, but being me, I wanted to prove it was feasible before offering it up. So I played around with some code, and a few hours later had a working prototype.

This blog post will look at the code for that prototype. It's important to remember that this code is not tested and follows none of the key design principles. Really it's awful code in all but the most important aspect; it works!

# The Idea

Regular readers will remember that on a few occasions, I have shown you domain models of code I was working on. If you think back to the [airport challenge post]({{ site.url }}/Airports), we had a domain model that looked a little like this:

![My helpful screenshot]({{ site.url }}/images/airport_domain.jpeg)

My idea for a useful Dev Tool was to write a ruby gem that analyses your code and automatically generates a domain model for your classes. Pretty Good Idea Right?

# Implementation

Before we look at the code, let me remind you that this is an awful implementation. The only reason I'm writing this blog post is to show some of the interesting code snippets that grew from this project, and to highlight that it's ok to spike code to understand a problem better. However, that code should never reach production; ideally it should be deleted and test driven from the ground up.  

Ok proviso's noted. Let's look at some code. I've created a barebones model of the boris bikes system to test our system on. It just has two classes, docking stations and bikes:

```ruby
class DockingStation

  def initialize(capacity)
    @bikes = []
    @capacity = capacity
  end

  def add_bike
    bike = Bike.new
    if bike.working?
      @bikes << bike
    else
      raise 'Bike Broken'
    end
  end

end


class Bike
  def initialize
    @working = true
  end

  def working?
    @working
  end

  def report_broken
    switch_state
  end

  def fix_bike
    switch_state
  end

  private

  def switch_state
    @working = !@working
  end
end
```

## Finding the Classes

Our first challenge is to read in the file, and find the classes in it. We'll use a command line argument to get the name of the file we want to analyse, which we can access by using `ARGV[0]`.

To start we require the file we're analysing. This loads the classes we want to analyse into memory. We then open the file as a text file, and scan it for any class definitions. This gives us an array of string holding the name of the classes in the file.

Then for each string we look up the actual class object in the ruby memory. These are stored as constants in the `Object` object. This gives us an array of all the classes we want to analyse.

```ruby
require(ARGV[0])

file = File.open(ARGV[0])
  @classes = file.read.scan(/class (\w+)/).flatten
file.close

@objects = @classes.map do |className|
  Object.const_get("#{className}")
end
```

## Probing the classes

Right, we've got our class objects. Our next step is to find out what methods and attributes they hold. We can use a few core ruby methods to do this.

When we find the public methods and private methods, we compare the method list to those defined on the `Object` class. As our classes will all inherit from `Object`, this will leave us with only those methods defined on our class.

```ruby
def find_attributes(object)
  object.instance_variables
end

def find_public_methods(object)
  object.public_methods - Object.new.public_methods
end

def find_private_methods(object)
  object.private_methods - Object.new.private_methods
end
```

If we write a little script to display this information in a nice format, we can now pass in our Boris Bikes script and get something like the following:

```
========================================================
CLASSES:
--------------------------------------------------------
DockingStation
  - Attributes:
    * @bikes
    * @capacity

  - Public Methods:
    * add_bike

  - Private Methods:

--------------------------------------------------------
Bike
  - Attributes:
    * @working

  - Public Methods:
    * working?
    * report_broken
    * fix_bike

  - Private Methods:
    * switch_state

--------------------------------------------------------
```

This gives us the information to display the nodes on our domain model. However the real strength of these maps is that display the relationships between nodes. To do this we need to delve deep into ruby.

## Set_Trace_Func - Recording Ruby Method Calls

Our approach for looking at class dependencies is to iterate through all our classes, calling each method in turn. We then record all the methods that are executed due to to this method call.

In this record we look for method calls where the object involved is different from the object we originally called the method on.

To record all method calls we use the Set_Trace_Func. This oft overlooked method takes a proc as an argument and allows us to record everything that ruby does.

Lets set it up to add a record to an array every time its invoked. For every public_method in a node we turn on the recorder, then call the method on the nodes class. We then turn off the recorder.

```ruby
node.public_methods.each do |method|
  set_trace_func proc { |event, file, line, id, binding, classname|
    @vertices << [node.classname, method, event, file, line, id, classname]
  }
    node.classname.method
  set_trace_func(nil)
end
```

Even a really simple program returns a huge number of ruby processes. Here's a sample from running our Boris Bikes program.

```[[DockingStation, :add_bike, "c-return", "DOM_Modeller.rb", 78, :set_trace_func, Kernel], [DockingStation, :add_bike, "line", "DOM_Modeller.rb", 81, :find_vertices, NodeMapper], [DockingStation, :add_bike, "call", "DOM_Modeller.rb", 21, :send, MethodSender], [DockingStation, :add_bike, "line", "DOM_Modeller.rb", 22, :send, MethodSender], [DockingStation, :add_bike, "c-call", "DOM_Modeller.rb", 22, :instance_method, Module], [DockingStation, :add_bike, "c-return", "DOM_Modeller.rb", 22, :instance_method, Module], [DockingStation, :add_bike, "c-call", "DOM_Modeller.rb", 22, :arity, UnboundMethod], [DockingStation, :add_bike, "c-return", "DOM_Modeller.rb", 22, :arity, UnboundMethod], [DockingStation, :add_bike, "line", "DOM_Modeller.rb", 23, :send, MethodSender], [DockingStation, :add_bike, "c-call", "DOM_Modeller.rb", 23, :new, Class], [DockingStation, :add_bike, "c-call", "DOM_Modeller.rb", 23, :initialize, Array], [DockingStation, :add_bike, "c-return", "DOM_Modeller.rb", 23, :initialize, Array], [DockingStation, :add_bike, "c-return", "DOM_Modeller.rb", 23, :new, Class], [DockingStation, :add_bike, "line", "DOM_Modeller.rb", 24, :send, MethodSender], [DockingStation, :add_bike, "c-call", "DOM_Modeller.rb", 24, :new, Class], [DockingStation, :add_bike, "call", "/Users/Tom/Programming/MakersAcademy/WeekNine/DOMTest/TestFiles/TestFile.rb", 3, :initialize, DockingStation], [DockingStation, :add_bike, "line", "/Users/Tom/Programming/MakersAcademy/WeekNine/DOMTest/TestFiles/TestFile.rb", 4, :initialize, DockingStation], [DockingStation, :add_bike, "line", "/Users/Tom/Programming/MakersAcademy/WeekNine/DOMTest/TestFiles/TestFile.rb", 5, :initialize, DockingStation], [DockingStation, :add_bike, "return", "/Users/Tom/Programming/MakersAcademy/WeekNine/DOMTest/TestFiles/TestFile.rb", 6, :initialize, DockingStation], [DockingStation, :add_bike, "c-return", "DOM_Modeller.rb", 24, :new, Class], [DockingStation, :add_bike, "line", "DOM_Modeller.rb", 25, :send, MethodSender], [DockingStation, :add_bike, "c-call", "DOM_Modeller.rb", 25, :instance_method, Module]
```
