---
title: Taking to the skies
excerpt: "In this blog we explore the a test driven solution to modelling an airport."
header:
  overlay_image: plane.jpg
  overlay_filter: 0.5 # same as adding an opacity of 0.5 to a black background
---
## Introduction
I had planned to explore a calculator program I wrote this week, but instead I want to focus on the first weekend challenge for Makers Academy.

The task asks you to develop the software to control the flow of planes at an airport.
The challenge repository can be found here: [Challenge Repository](https://github.com/makersacademy/airport_challenge).

My solution can be found here: [Solution](https://github.com/TomStuart92/airport_challenge).

## System Design

When tackling these sorts of tasks the best place to start is with the user stories. These should give you a framework from which to build your project. From this we can develop domain models and then start using Test Driven Development to write our software.

Lets look at the user stories we're provided with:

```
As an air traffic controller
So I can get passengers to a destination
I want to instruct a plane to land at an airport and confirm that it has landed

As an air traffic controller
So I can get passengers on the way to their destination
I want to instruct a plane to take off from an airport and confirm that it is no longer in the airport

As an air traffic controller
To ensure safety
I want to prevent takeoff when weather is stormy

As an air traffic controller
To ensure safety
I want to prevent landing when weather is stormy

As an air traffic controller
To ensure safety
I want to prevent landing when the airport is full

As the system designer
So that the software can be used for many different airports
I would like a default airport capacity that can be overridden as appropriate
```

Reading through we can break these down into nouns and verbs. The nouns represent objects which we will build into classes. The verbs represent methods that we use to pass messages between the classes.

All our user stories start with 'As a ...', but for our purpose these will represent the user. So the objects we will need to build classes for are Plane, Weather, and Airport.
The verbs we have are land, takeoff, and prevent. Prevent will encompass a whole host of conditions that we will need to check.

From this we can begin to imagine the structure of our code. We will have a weather class which is responsible for determining if it is safe to fly. We will have a Plane class than can land and takeoff. Finally we will have a Airport class which will be responsible for carrying out our prevention checks, and then issuing the correct land and takeoff commands. The Domain model below shows the interactions between our classes:

![My helpful screenshot]({{ site.url }}/images/airport_domain.jpeg)

## The Weather Class
Lets start with the Weather class. According to our domain model, it needs to have one public method, check_safe? that should return a boolean (True or False) based on the current state of the weather. The requirements as laid out in the challenge, mean we will have to use a random number generator to govern this behaviour.

Following the tenets of Extreme and Agile Programming, lets start by writing some tests. We use Test Driven Development to drive our code design, ensuring we're meeting the system requirements, but no more. We'll use the Rspec testing framework to do this. Our tests need to do three things. Check the weather updates, checks that check_safe? returns false if the weather is not safe, and returns true if the weather is safe:

In Rspec:

```ruby
require 'weather'
describe Weather do
  subject(:weather) {described_class.new}

  context 'behaviour' do
    it 'should update to a new weather state randomly' do
      acc = []
      1000.times do
        acc << weather.check_safe?
      end
      expect(acc.uniq.length).to be > 1
    end

    it 'should return unsafe to fly if weather is unsafe' do
      allow(Kernel).to receive(:rand).and_return(1)
      expect(weather.check_safe?).to be false
    end

    it 'should return safe to fly if weather is safe' do
      allow(Kernel).to receive(:rand).and_return(10)
      expect(weather.check_safe?).to be true
    end
  end
end
```
The first test checks the weather 1000 times and checks that the result changes at least once. While this test isn't entirely waterproof, as long as we choose a probability over 10% for bad weather, the chance of receiving true 1000 times in a row becomes so small, we can ignore it.

The next two tests check our check_safe? method returns the correct boolean. Here we are stubbing the random number generator. We do this to make sure the weather is either good (10) or bad(1). We then expect the correct return value.

Now we've written our tests we can move on to the actual code. Given this class has only one public method, it is relatively easy to write:

```ruby
# Creates a probabilistic weather model.
class Weather
  def initialize(unsafe_prop = 2)
    @unsafe_prop  = unsafe_prop
  end
  def check_safe?
     Kernel.rand(1..10) > @unsafe_prop
  end
end
```
This is quite a basic implementation, but it serves our purposes. We can pass it an argument to set the level at which bad weather is returned. This is defaults to 2 if we provide no argument. We then check if a random number from 0 to 10 is greater than our `unsafe_prop` value. If it is, we treat the weather as safe, if not we treat the weather as unsafe.

With this code, our tests pass! We can now move on to our plane class.

## Plane Class

Our plane class is similarly built test first. For brevity I'll leave out the tests and just present the code, so we can move on to the more interesting Airport class. Our Plane class tracks its state through the `@landed` instance variable. It has two public methods as we saw in our domain model, that updates this internal state.

```ruby
# Creates plane objects with internal state.
class Plane
  attr_reader :landed
  alias landed? landed

  def initialize
    @landed = false
  end

  def land
    @landed = true
  end

  def takeoff
    @landed = false
  end
end
```
To conform to Hound's style guide, we've used an attr_reader to return the value of `@landed`. This is the same as defining:
```ruby
  def landed
    @landed
  end
```
We use an alias, to allow us to call `.landed` as `.landed?` as it returns a boolean. Such methods end with question marks by convention. I feel there must be a better way of doing this. Let me know if you have a trick!

### Airport Class

So finally, we come to our airport class. Lets start with the landing functionality. Below are the tests for this. We create a plane double which behaves as a kind of fake plane. If we used our real plane class, an error there would make our airport tests fail, but here we only want to test the airport class. We tell the double to respond to the landed? method and return false, representing a flying plane.

We expect our doubles to receive land method calls and then test that our internal `@landed_planes` variable contains both planes. We also defend against edge cases. This is our prevent verb from our domain model. The system shouldn't land planes when stormy, so we stub out the random number generator again and test that we don't land the plane. It should also not land when full, so we create a second airport with a capacity of 0, and finally it should not land already landed planes.

```ruby
require 'airport'
subject(:airport) {described_class.new}
subject(:custom_airport) {described_class.new(capacity: 0)}

describe '#land(plane) operations' do
  before(:example) do
    @plane = double(:plane, landed?: false)
    @plane2 = double(:plane, landed?: false)
  end
  context 'in normal landing scenario' do
    it 'can land multiple objects' do
      expect(@plane).to receive(:land)
      airport.land(@plane)
      expect(@plane2).to receive(:land)
      airport.land(@plane2)

      res = [@plane, @plane2]
      expect(airport.instance_variable_get(:@landed_planes)).to eq(res)
    end
  end
  context 'defends against edge cases' do
  it 'will not land objects if weather unsafe' do
    allow(Kernel).to receive(:rand).and_return(1)
    expect(@plane).to_not receive(:land)

    error= 'Poor weather means the plane has to divert.'
    expect { airport.land(@plane) }.to raise_error(error)
  end
  it 'should not land objects when over capacity' do
    expect(@plane).to_not receive(:land)

    error = 'Airport is full, the plane has diverted.'
    expect {custom_airport.land(@plane)}.to raise_error(error)
  end
  it 'should prevent landed planes from landing again' do
    allow(@plane).to receive(:landed?).and_return(true)
    expect(@plane).to_not receive(:land)

    error = 'That plane is at another airport.'
    expect {airport.land(@plane)}.to raise_error(error)
  end
end
end
```

Having written the code we can then turn to writing our airport class code:

```ruby
class Airport
  DEFAULT_CAPACITY = 6

  def initialize(options = {})
    @capacity = options.fetch(:capacity, DEFAULT_CAPACITY)
    @weather = options.fetch(:weather_system, Weather.new)
    @landed_planes = []
  end
end
```
We start with our initialisation method. We've used a options hash here as something recommended in Sandi Metz' Practical Object Oriented Design. This is a book I would definitely recommend for those looking to make the jump from the syntax of making classes, to the best practices of making good classes. Using a hash, removes the dependency on argument order, which is created when methods need multiple arguments. We also use dependency injection for the weather class, so we can use the airport with any other class than responds to the .check_safe? method. This again brings flexibility to our code.

We also use this method to allow us to define the custom capacity that we need to satisfy the last of the user stories. If no custom capacity is given it defaults to a capacity of six.

```ruby
#continued from above...
  def land(plane)
    pre_landing_checks(plane)
    plane.land
    add_plane_to_airport(plane)
    puts "The plane has landed safely at the airport."
  end

  private

  attr_reader :capacity, :weather, :landed_planes

  def pre_landing_checks(plane)
    fail "That plane is at another airport." if plane_landed?(plane)
    fail 'Airport is full, the plane has diverted.' if airport_full?
    fail 'Poor weather means the plane has to divert.' unless weather_safe?
  end

  def add_plane_to_airport(plane)
    landed_planes << plane
  end

  def plane_landed?(plane)
    plane.landed?
  end

  def weather_safe?
    weather.check_safe?
  end

  def airport_full?
    landed_planes.size >= capacity
  end
end
```
The land method delegates to several other methods to try to give them each a single responsibility. To being we run pre_landing_checks which is responsible for carrying out the `prevent` checks. The first checks if the plane is landed by using the .landed? method we defined on the plane class. We then check if the airport is full, by checking if the number of landed planes is equal to the capacity. Finally we check is the weather is safe, by calling the check_safe? method on our weather class. If any of these checks fail, they raise a runtime error.

If our checks pass, we call the plane.land method to update the internal state of the plane to landed. Finally we add the plane to the landed_plane array. With this code, our tests now pass. We repeat this process with the takeoff method, to arrive at our final implementation.

## Feature testing

Now we have our code we can feature test our system. If we require the file in irb using `irb -r './lib/airport.rb'`, we can then test:

```
tomstuamacbook2:airport_challenge Tom$ irb -r './lib/airport.rb'
2.2.4 :001 > airport=Airport.new
 => #<Airport:0x007fa00393aad0 @capacity=6, @weather=#<Weather:0x007fa00393aa80 @unsafe_prop=2>, @landed_planes=[]>
2.2.4 :002 > plane=Plane.new
 => #<Plane:0x007fa00403cf40 @landed=false>
2.2.4 :003 > airport.land(plane)
RuntimeError: Poor weather means the plane has to divert.
	from /Users/Tom/Programming/MakersAcademy/WeekOne/Airport/airport_challenge/lib/airport.rb:34:in `pre_landing_checks'
	from /Users/Tom/Programming/MakersAcademy/WeekOne/Airport/airport_challenge/lib/airport.rb:14:in `land'
	from (irb):4
	from /Users/Tom/.rvm/rubies/ruby-2.2.4/bin/irb:11:in `<main>'
2.2.4 :004 > airport.land(plane)
The plane has landed safely at the airport.
 => nil
2.2.4 :005 > airport.land(plane)
RuntimeError: That plane is at another airport.
	from /Users/Tom/Programming/MakersAcademy/WeekOne/Airport/airport_challenge/lib/airport.rb:32:in `pre_landing_checks'
	from /Users/Tom/Programming/MakersAcademy/WeekOne/Airport/airport_challenge/lib/airport.rb:14:in `land'
	from (irb):6
	from /Users/Tom/.rvm/rubies/ruby-2.2.4/bin/irb:11:in `<main>'
2.2.4 :006 > airport.takeoff(plane)
The plane has successfully taken off.
 => nil
```

Our completed code now fulfils all the requirements of our user stories. It can land and takeoff planes, defends against edge cases, and can be initialised with a custom capacity.
