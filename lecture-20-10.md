# Lecture 20/06

## Agile Methodology

The manifesto:
- individuals and interactions over / has more weight than processes and tools
    - talk as we are working on the project
- working software over comprehensive documentation
    - instead of wasting time negotation, start showing what you have, get the feedback loop going
    - clients change their minds after 18 months, avoid this!
    - the companies that wins are the ones that put things out faster
- customer collaboration over contract negotiations
- responding to change over following a plan

Inspired by Toyota Production System (TPS)

Philosophy of continuous delivery (12 principles):
- get feedback sooner than later from client, esp. when they can interact with it frequently
- no matter how small, it should be **working**
    - even if its just a button that goes somewhere
- sustainable pace of work, feel like we're making progress at a constant rate
- simplicity is essential, we can over-engineer anything
- self-organizing team: 5-8 people in a team, more is too much
    - the more people, the more connections, the more word-of-mouth communication there is
    - even big companies work on really small teams
    - do this based on interests and skillsets
- regular reflection on how to become effective and then adjusting accordingly

### 80/20 rule (Pareto principle)

Some italian noticed 80% of the land belongs to 20% of the people, apparently.

20% of your effort becomes 80% of the outcome, 80% of your effort is only 20% of the outcome.

To consider:
- Focus on 4 features that are really well done, rather than the 16 features that are mid
- Those four end up becoming your 80%

State design pattern related to a sprint board with:
- Story
- Todos
- Doing
- Done

Anyone taking a look at this knows where things are right away, no status meeting needed.

A/B testing: see if its a feature that is liked, then role it out to other places

### Methodologies

- Scrum
- Kanban
- Lean

### User stories

Needs to be readable by tech and non-tech people

INVEST:
- independent
- negotiable
- valuable
- estimatable
- sized appropriately
- testable

Acceptance criteria (very similar to testing language)
- given, when, then
- User Context, Event/Action, Solution/Outcome

"The less we have to manage, the better off we'll be"
- more manageable in theory for each prod release, deploy multiple times a day
- rant about productivity and timeliness lol, quality of work matters most

BIG DIG MENTIONED
- "imagine coming up with agile for the big dig"
- will be a mess if you have no planning and no waterfall
- some things make sense for agile, some things don't
- some things need more planning

Wow agile used to be a buzzword 20 years ago
- every department became agile.... booooo...

## State Design Pattern (B)

Behavorial design pattern

Problems when object has a lot of states and state changes

### The Problem

Object with different states

### Why

Easily (1) add state (2) remove states (3) change the transitions

Avoid code duplication and hard coding of condiitonal code

### Official definition

State design pattern allows an object to alter its behvaior when its internal state changes. The object will appear to change its class.

### Notes

State machine overview:
- state changes depending on input, transition to another input
- NTMs and DTMs <3
- State diagram! should have one if using state diagram
    - in addition to the UML diagram 

Coin-operated turnstile example:
- if enough money, allow thru
- else, locked
- why does prof think trains and buses still do this lol
- 2 states: locked and unlocked (for state diagram)
- actions: coin and push as arrows, see how transition happens

> Theoretically: if we increase the price, then the "noCoinState" is going to be the one responsible for keeping the counter of how many coins inserted

State objects are usually **Singletons**.
- we do not need more than one state object

Every state is responsible for its own transition

### Implementation

Define state **interface** that contains a method for every action of your state machine

Implement a state class for every state of the machine, with behavior of that state

Remove all conditional code and instead delegate to the State class (BUT this does not mean it is a structural design pattern)

### Example

Vending machine example

State Interface (abstract) - stuff that ALL states need to have (transitions):
- insertCoins
- ejectCoins
- pushButton
- pushDispense
- refill

ex. HasCoinState:
- insertCoin -> cannot insert more coins!
- ejectCoins -> works, then sodaMachine.setState(sodaMachine.getNoQuarterState())

SodaMachine (concrete) - uses the state:
- insertCoin -> state.insertCoins() - calls the state function, meaning it removes the conditional layer!

### When to use

Object behavior that depends on its state and must change behavior

Large multiple-part conditional statements

Organize your logic in each state

### Consequences

- Logic is organized into each state
- New state can be easily added
- New state means new subclasses
- State transitions are explicit
- State objects can be shared

related: Flyweight pattern (won't cover), Chain of Responsibility (won't cover), Decorator

### In-class Example

Agile ticket states!
- TODO
- Doing
- Done

State interface
- startWorkingOn
- finishWorkingOn
- stopWorkingOn

UserStory object:
- initialize all states and pass in self
- set initial state

## Adapter Design Pattern (S)

Structural design pattern

Intro: those USB adapter wires (usb to hdmi), makes things compatible

Problem: Making incompatible things compatible

### Official Definition

Convert interface of a class into another interface that another class expects

### Implementation

#### (1) Object Adapter

Target interface (with request())

Adapter implements Target (with request())

Adapter invokes Adaptee (with specificRequest())

Client just invokes request()

#### (2) Class Adapter

For multiple inheritance (python and cpp), adapter class is a subclass of both!

### Notes

Use **composition** to wrap an existing object and provides a new interface

> Composition (has-a) vs Inheritance (is-a)?

Separation of concerns
- layers of code
- pattern helps separate concerns of client and adaptee

Related patterns: decorator, bridge, proxy, facade

### Example

interface UsbCharger
- charge()

ComputerCharger implements UsbCharger
- overrides charge()

interface CigaretteLighterCharger
- chargeViaCarLighter() <- this is a specific method thats required, can't be changed

FerarriCharger
- overrides chargeViaCarLighter()

The ADAPTER: UsbToCigaretteLighterAdapter concrete class
- implements UsbCharge (with the request that the client wants!), the male
- big punchline: We can use the same API charge()

Compatible interfaces are very powerful!

## Proxy Pattern (S)

Structural pattern

What is the problem?
- Provides a placeholder for another object to **control access to it**
- Clients need to access the same object multiple time and control access can help (ex. save costs, some protection)

Stock market example:
- Client makes request to proxy, proxy sends request to real subject
- After 4 PM when market closes, proxy surfaces last known prices instead, instead of wasting resources doing a lookup

### Structure

Client does not know if they are talking to RealSubject or Proxy, both implements from the same interface Subject

Simple UML!

### Notes

Types of proxies:
- remote proxy
- virtual proxy
- protection proxy
- caching proxy

Examples with CDNs and where you are accessing from (if coming in from too far), fast access from any where

- Synchronization proxy
- Complexity hiding proxy (facade pattern!)
- Firewall proxy
- so many

Can be used when an object is **expensive to create**
- singletons NOT the same! whether or not it is expensive to create doesn't matter!

## Decorator Pattern (S)

Structural pattern

Summary:
- imagine a christmas tree (main object)
- we can add ornaments, snow, star, etc. (decorating the christmas tree)
- but we don't change the base object

Problem:
- assign extra duties to a specific object without including them in an entire object class
- ability to add or remove responsibilities from the object during runtime
- too many subclasses! we don't want too many subclasses!
- **same functionality without having to make another subclass**

Is inhertiance the enemy?
- it should be the last resort
- prefer composition (has-a) over inheritance (is-a)

### Example

Coffee Machine with so many subclasses to beverage (different combos of milk and sugar)

Endgoal: decorate the beverage with milk

```java
Beverage beverage = new Espresso();
Beverage latte = new Milk(beverage);
// can chain this...?
```

Meaning we didn't have to create a whole new class for latte

### When to use

prevent large number of subclasses

remove responsibilites when necessary

all dynamic! meaning latte is only created at run-time, instead of the structure having to be built out beforehand

Greater flexibility compared to inheritance

Complexity can go up, size of classes can go up.
- Regarding complexity: Don't implement if no one else knows it, bc that'll just increase complexity. It's really only nice when everyone else knows it.
