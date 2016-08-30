---
title: Sorting Spaceships
excerpt: "In this blog post we explore the spaceship operator."
teaser: spaceship.jpg
header:
  overlay_image: spaceship.jpg
  overlay_filter: 0.5 # same as adding an opacity of 0.5 to a black background
---
## Sorting Objects

We all know that we can sort a variety of ruby objects. Sorting relies on us being able to define one object as becoming before or being bigger (>) than another:

For example

`1 < 3 == true`

`'a' > 't' == false`

These kind of operations allow us to sort collections of these objects using the .sort method:

```ruby
[5, 2, 4, 8, 10].sort = [2, 4, 5, 8, 10]

['t', 'a', 'n'].sort = ['a', 'n', 't']
```
All the sort method does is take each pair in turn, and order them by which is bigger using the > operator.

## How it works!

Every class that can be sorted including String and Integer have a module mixed in called Comparable.
This module allows us to define how instances of that class are sorted:

- In the Integer class this is simply the 1 < 10 etc that we are all familiar with.
- Comparing Strings sorts them alphabetically.

But what about for other classes?
Luckily we can add this module to our own classes and then we can sort them!

Lets look at our Oystercard class.
For this exercise lets say that a 'bigger' Oystercard has a bigger balance.

What happens if we try to compare two instances without doing anything?

```ruby
2.2.4 :001> class Oystercard
2.2.4 :002>   attr_reader :balance
2.2.4 :003>   def initialize(balance)
2.2.4 :004>     @balance = balance
2.2.4 :005>   end
2.2.4 :006> end    
 => nil
2.2.4 :007>
2.2.4 :008> big_card = Oystercard.new(1000)
 => #<Oystercard:0x007f80b3028678 @balance=1000>
2.2.4 :009> little_card = Oystercard.new(1)
 => #<Oystercard:0x007f80b20a8fe0 @balance=1>
2.2.4 :00> big_card > little_card
NoMethodError: undefined method '>' for #<Oystercard:0x007f80b3028678 @balance=1000>
	from (irb):15
	from /Users/Tom/.rvm/rubies/ruby-2.2.4/bin/irb:11:in '<main>'
```
We get an undefined method error! We haven't told ruby how to compare two instances with the > operator.
It doesn't know what it means for one Oystercard to be bigger than another!

To do this we need to introduce perhaps the best named operator of all time...

THE SPACESHIP OPERATOR: <=>

## To Infinity and Beyond

The spaceship operator is how we define which of two objects is 'bigger'.

With classes like Integer this is obvious (10 > 1), but with other classes less so.

For our oyster card class, lets define the 'bigger' oyster card to be the one with the bigger balance.

We write the spaceship operator for this as below:

```ruby
def <=> another
  self.balance <=> another.balance
end
```
On line one we've written `<=> other`.
This may seem a little odd but think of it as similar to the shovel operator (`[] << 1`).
When this method is called it will look like this: `object1.<=>(object2)`.

The self in the second line is the first object we compare, the 'another' is the second.
In X > Y, X is the self, Y is the another.

We've told it to compare two attributes of the objects we want to compare.
Both of these attributes are Integers.
Ruby already knows how to compare integers so it can work out which object is bigger.

## Adding Sorting to Classes

So if we want to be able to sort Oystercard instances, there are two things we need to do:

- Include the module
- Define a spaceship operator

```ruby
class Oystercard
include Comparable   #Include Module
attr_reader :balance
  def initialize(balance)
    @balance = balance
  end

  def <=> another      #Define Spaceship Operator
    self.balance <=> another.balance
  end
end
```
So trying our IRB again:

```ruby
2.2.4 :023 > class Oystercard
2.2.4 :024?>   include Comparable
2.2.4 :025?>   attr_reader :balance
2.2.4 :026?>   def initialize(balance)
2.2.4 :027?>     @balance = balance
2.2.4 :028?>   end
2.2.4 :029?>   def <=> another
2.2.4 :030?>     self.balance <=> another.balance
2.2.4 :031?>   end
2.2.4 :032?> end
 => :<=>
2.2.4 :033 > big = Oystercard.new(1000)
 => #<Oystercard:0x007f80b38d28f0 @balance=1000>
2.2.4 :034 > little = Oystercard.new(1)
 => #<Oystercard:0x007f80b38c36e8 @balance=1>
2.2.4 :035 > big > little
 => true
2.2.4 :036 > [big, little].sort
 => [#<Oystercard:0x007f80b38c36e8 @balance=1>, #<Oystercard:0x007f80b38d28f0 @balance=1000>]
```

## Other types of Spaceships

In the example above, we used the fact that we wanted to compare two integer attributes. This let us use the integer comparison spaceship.
But what if we wanted to compare something unrelated?

Lets say we want to create a Coin class, where a `:head` is 'bigger' than a `:tail`:

```ruby
class Coin
include Comparable
  attr_reader :state
  def initialize(state)
    @state = state
  end

  def <=> another
    ????
  end
end
```
How do we define `:head` > `:tails`?
It turns out all we really need to return from the spaceship operator is either -1, 0 or 1.

```ruby
A <=> B
  return 1 #means A is bigger than B
  return 0 #means A is equal to B
  return -1 #means A is less than B
```
So we can use a series of ifs to define our bigger than statement.
If both coins are the same we return 0.
Otherwise, if I have a head, I am bigger - return 1.
Otherwise, I have a tails, I am smaller - return -1.

```ruby
def <=> another
  return 0 if self.state == another.state
  return 1 if self.state == :heads
  return -1 if self.state == :tails
end
```
So including this in our class definition:

```ruby
2.2.4 :001 > class Coin
2.2.4 :002?> include Comparable
2.2.4 :003?>    attr_reader :state
2.2.4 :004?>    def initialize(state)
2.2.4 :005?>      @state = state
2.2.4 :006?>    end
2.2.4 :007?>    def <=> other
2.2.4 :008?>      return 0 if self.state == another.state
2.2.4 :009?>      return 1 if self.state == :heads
2.2.4 :010?>      return -1 if self.state == :tails
2.2.4 :011?>   end
2.2.4 :012?> end
 => :<=>
2.2.4 :013 > heads = Coin.new(:heads)
 => #<Coin:0x007fbcee0a5660 @state=:heads>
2.2.4 :014 > tails = Coin.new(:tails)
 => #<Coin:0x007fbcee05dbd0 @state=:tails>
2.2.4 :015 > heads > tails
 => true
2.2.4 :016 > [tails, heads].sort
 => [#<Coin:0x007fbcee05dbd0 @state=:tails>, #<Coin:0x007fbcee0a5660 @state=:heads>]
```
We have implemented a custom sort based on two symbols!
