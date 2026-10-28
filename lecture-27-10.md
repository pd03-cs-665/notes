# Lecture 27/10

## Final project

Come up with problem that interests you

Implement a pattern covered in class + a pattern that we haven't discussed yet

Should be your best code! Effort matters.

Due Dec 8th-ish

Whatever you present should be what you submit

Same scope-ish as the assignment (3-5 files is too small)

Make sure you use the project template. Stick with plain Java, no frameworks.

Need both slides AND demo in the IDE. Have motivation and inspo section.

Quality > quantity

## Wrapping up Decorator Pattern

Reminder: decorator pattern is used to prevent too many subclasses from happening
- Add new features by decorating that object

### Example

Abstract class Car with abstract method getCost

Create concrete class HondaAccord. This will be what we are decorating (i.e. add features without creating subclasses)

Abstract FeatureDecorator that extends Car (is-a car and also has-a car)

Features will extend FeatureDecorator (i.e. LeatherSeat extends FeatureDecorator, Sunroof extends FeatureDecorator)
- includes both is-a and has-a relationship
- the init method takes in Car
- expected methods change, i.e. getCost is added

Final result is that

```java
Car sportyCar = new HondaAccord()
sportyCar = new Sunroof(sportyCar)
sportyCar = new LeatherSeat(sportyCar)

// or use chaining

Car sportyCar = new LeatherSeat(new Sunroof(new HondaAccord()))
```

## Composite Pattern (S)

Structural pattern

In trees, composite nodes are nodes that contains other nodes (has children, that aren't leaves)

> The more things we have to manage, the worse off we'll be!

We want to treat all nodes (composites or leaves) the same. No code duplication or inefficiency.

Formal definition: Composite pattern composes objects into tree structures to represent part-whole relationship.

### Example

All menus -> Dinner Menu, Lunch Menu, Drink Menu, etc. (`composites`) -> each menu has menu items (`leaves`)

We want to perform an operation that treats all of these items the same.

Problem: if we want to print only the drink menu items, we'll run into problems, like checking if each menu item is a drink, which will fail for other menus...

### Partipants

Component: abstract class as common interface for all objects in the composition

Leaf: leaf object w/ no child components

Composite: class w/ composite objects, which can contain other composite or leaves

Client: interacting with stuff above

### When to use?

Representing **part-whole hierarchies** and client needs to manipulate object uniformly

### Consequences

Treat primitive and composite objects uniformly

Drawback: too general, hard to restrict components of a composite

### Code example

Basic interface Car with print() method

Ferrari implements Car and overrides print

Honda implements Car and overrides print

Lambo implements Car and overrides print... etc.

*MyCars implements Car* but contains an ArrayList of Car childCars
- overridden print loops over childCars array list and prints it out
- additional methods: addCar and removeCar

Emphasis is that all of the above has the same API `.print()` regardless if it is a container of other cars

## Command Pattern (B)

Behavioral pattern

### Javascript tangent

Callback: passing function to a function `sayHello(sayGoodbye)` where sayGoodbye is the callback and invoke it in the function. We can "call back" a function whenever we want.

> Function as first-class citizen, meaning function is treated like any other data types

### Summary

We want to treat commands the same uniformly, we can execute and pass them whenever we want

Basic concept is being able to pass a function as an arg

Formal definition: Encapsulate a request as an object, thereby lettign you parameterize different requests, queu or log request, and support undoable operations

Real example: cmd+z undo command works no matter what key you pressed to undo, operation is undone bc its the same api

### Example

Base interface Command with execute() and undo()

We can keep a history stack of all commands executed, call undo() on each cmd+z, etc.

### When to use

Anything with Callable related stuff I guess

- undoable or redoable operations
- decouple the invoker from the receiver, allowing them to vary independently
- implementation for logging and retry logic?
- since we have the stack of cmds, we only add the command when its successsful... and in the catch block is where the "retry" logic is

### Code example

Lighting system

Base interface Command with execute method

Light concrete object with on/off methods

DecreaseLightCommand implements Command with Light theLight reference. Overrides execute() method to call the turn off method.

Switch concrete object to keep track of List<Command> history and storeAndExecute method

Always remember fundamentals and intentions, rule of thumbs, etc.

## Mediator Pattern (B)

Behavioral pattern

The problem: objects in a system are too tightly coupled when they directly and explicitly reference each other, meaning it is harder to maintain, extend, and scale.

Big picture: problem with too many planes trying to land, plane should talk to the tower (mediator) for concrete answer for when to land, instead of talking to each other.

Mediator pattern has a many-to-many relationship (remember, observer is one-to-many)
- the plane can talk to all the other planes
- the fact that the plane can talk to the tower is what makes it different
- "observers" vs "colleagues"

Definition: an object that encapsulates how a set of objects interact, promotes loose coupling by keeping objects from referring to each other explicitly

Napster mentioned, peer-to-peer and torrenting system, where in mediator there is a centralized place

In the "colleague" class, at the action method (setMyOffer), call mediator to notify every one else.
