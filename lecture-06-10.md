# Lecture 06/10

## Abstract Factory Pattern (C)

Next level from the factory pattern

More about creating families of objects w/o specifying concrete types
- Different types or super types

Less "applicable" than the factory pattern

Pros:
- Consistency across products

Cons:
- More class files
- Supporting new kinds of products is difficult

### Example

Car example... UK and US factories, same "Car" blueprint but different "families"

Pizza example again

Each ingredient is an interface (Cheese, Clams, Dough, Pepperoni, Sauce, Veggies)

More specific types returns specific cheese, doughs, sauces, etc.

Abstract class (class that can't be instantiated) pizza
- abstract prepare method
- void bake(), cut(), box() concrete methods
- setName() and getName()

Cheese pizza extends the pizza, then instantiate PizzaIngredientFactory interface

Pizza store abstract class
- abstract createPizza() method
- orderPIzza() concrete class

Same ingredients but different

Abstraction is the ingredient factory -- the dough, cheese, etc. are abstract, but then needs to be specific when creating the pizza.

This example is complicated... don't implement patterns just bc it looks cool. Make sure there's a real benefit.

Factory method by itself is useful for any application that creates objects, that's why it might be more common.

Review. Intention is a big piece:
- Observer pattern when subscribers want to get notified on state changes
- Strategy pattern when inheritance is not our friend, siblings cannot borrow code from each other
- Factory method when determining what classes to create at run time

## Iterator Pattern (B)

Behavioral pattern

Problem:
- When we want to iterate over a collection
- Code can be tightly coupled to the collection / data structure type (custom data structures want to use the same iterator)

Iterator:
- next()
- hasNext()

Aggregate (uses Iterator):
- instantiates iterator()

Multithreading
- Iterator will help with thread safety
-  ex. Guy and Wife takes $1000 out of bank account (with only $1000) at the same time. This shouldn't happen. (How does iterator pattern solve this...?)

Pros:
- Simplifies client code
- decouples collection and iteration (separation of concerns)
- Multiple iterators to operate over the same collection without interfering with each other
- Lazy evaluation (only computed when fetched)

Cons:
- Adds overhead

### Example

Interface Cars with an iterator createIterator()

Interface Iterator
- hasNext() returns boolean
- next() returns Object (can return anything)


Concrete class **EconomyCars** implements Cars
- cars ArrayList<string>()

Because we use ArrayList, we can use:
- addItem(carName) with built in cars.add(carName)

Random review, why do we implement toString() methods?
- we want a pretty string repr of custom object, not the default memory address print


Concrete class **FancyCars** implements Cars

FancyCars constructor:
- String[] cars
- cars = new String[MAX_NUMBER]
- addItem() can check max and then add to cars[numberOfCars] list (and then numberOfCars++)

So for these two classes, we have an ArrayList and then a regular list String[]... two different data structures!!

In the main class:

### WITHOUT iterator pattern

Looping over fancycars, we have to know list.cars.length, and then bracket notation [] to print out each car

And then for economycars, we also have a for loop, but then we have to use size() and get() specific for ArrayList

This is bad business! Have to manage 2 APIs instead of 1.

> Me: I guess, I know you set this up for the sake of the example, but if I were to see this in the real world, my instinct would be to get all the children of Cars to use the same data collection type, rather than use the Iterator pattern. So this makes me feel like seeing an Iterator pattern might be the code smell itself?


ANSWER: Array vs ArrayList

Array:
- fixed size
- old school
- a lot faster
- O(1)

ArrayList:
- dynamic size (kinda, increases size when halfway full)
- newer ("cooler")

So theres size and memory tradeoff. But prof is with me that switching to ArrayList is the better option here (for cars example) than implementing the Iterator pattern.

### WITH iterator pattern

Main method has printCars() with the iterator passed in

```
new FancyCars().createIterator()
new EconomyCars().createIterator()
```

Where we now create EconomyCarIterator implements those details of hasNext() specific to ArrayList

Same for FancyCarIterator where the details are specific to the String[] type.

## Singleton Pattern (C)

Most popular design pattern

There's also multi-ton (restrict to N number of objects)

Creational pattern

When we want only one instance of the class to exist

Restrict client to only ever work with one instance. If they need it, they should use the one that currently exists.

VERY IMPORTANT: Benefit is **NOT** efficiency!! (Not the main point, even if its true). The main benefit SHOULD just be, only have one instance instatiated once.

> **static** keyword, means method belongs to class and not the instance (AKA @staticmethod in Python)

Constructor method is PRIVATE. No one should mess with this.

getInstance() to check if uniqueInstance is created, else returns that unique instance. Method has to be static. uniqueInstance also must be static.

"Static Initialization": creates new Singleton() right at startup. Even if you don't call it, you're always creating it.

"Implementation": null check during getInstance() then created there.

Consequences:
- Controlled access
- Avoids polluting code with global variables (apparently, they're evil)
    - If it changes, it affects everything. **Scoping** is the issue.
    - This is different from global constants
    - Variables should be exposed with accesss rights (need-to-know-basis). Not direct competition with Singletons, but something to highlight.
    - In design, put variables as close as possible to where it is used.
- We can change our mind on Singletons and allow more instances (important in design in general)

Thread-safe implementation of Singleton:
- We do not want getInstance() to be ran in parallel, so use `synchronized` keyword to properly lock method (since `getInstance` could create an instance, we dont want multiple instances by accident)

### Example

DatabaseConnection class

Private static DatabaseConnection onlyNeedOne (default to null at runtime)

Private constructor that does nothing (only job is that no other class can instantiate a new instance)

## Multiton Pattern (C)

This is in the Singleton Pattern slides.

Set to N objects created

Lol nothing else to say about this

## Facade Pattern (S)

Structural pattern

Big picture:
- Nice picture of a building
- What matters: we don't see plumbing details, electrical wiring details, other complexities, etc.
- Goal is abstraction and removing complexities

Unified interface to a set of interfaces in a subsystem (easier to use = abstraction)

Instead of knowing all the complexities, client calls one entry point (the facade) that will handle calling all the other dependencies.

Participants:
- Facade: knows which subsystem classes are responsible and is needed, delegates client requests to appropriate subsystem objects
- Subsystem Classes: does NOT know about the facade, handles the work assigned only

IMPORTANT: Facade is there only for abstraction. Subsytem should work fine without it.

Generally, only one facade object is needed (is a singleton)

When to use:
- Single interface for a complex system

Pros:
- If we want to update subsystem, facade doesn't need to be changed, nothing breaks for the client
- Decoupling
- Can reduce compilation
- Does NOT prevent client from using the subsystem

Cons:
- Its good to know how subsytems work (personal feelings)
    - Prof does not like magical things LOL, things go wrong and having to open the hood. Good to know how things work.
- When things go wrong, tracing (could) be painful

Quote: "I can kill a fly with a bazooka. Doesn't mean I should."

### Example

Smart Home facade

Want to hit only one button that will set up your home (turn lights on, turns TV on, etc.) that will execute a bunch of methods
