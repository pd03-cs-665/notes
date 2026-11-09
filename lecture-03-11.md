# Lecture 03/11

## class logistics

final exams in-person on paper, cheatsheet allowed

Given cheatsheet, in general remember pattern's intentions

## Visitor Pattern (B)

Behavioral pattern

Problem: We want to perform different operations on objects of class hierarchy, but adding new ones require modifying base class, where things can go wrong.

IRL example: we don't want to give client source code, we give client the .class executable code. Client wants to tweak something, makes expensive request. So problem is that you don't have access to source code, but want to make changes.

Open backdoor to software so client can inject code in our codebase. AKA client "visits" my code even though they don't have access to it.

Almost sounds like decorator pattern, i.e. adding and modifying behavior at object at run time.

Formal definition: Represent an operation to be performed on elements of an object structure. Visitor lets you define a new operation without changing the classes of the elements on which it operates.

### Consequences

- allows adding new operations to an object hierarchy w/o modifying object themsevles
- keeps related code together
- allow separation of concerns as we add operations to a complex object
  - MVC model-view-controller mentioned
  - microservices
  - one function does one thing and does it very well

### Example

Car interface w/ getSpeed(), goFast(), and void accept()
- the void accept() function is a "backdoor" symptom where we can take in an object carVistor where we can inject things

Concrete Honda class implements Car, where accept(carVisitor) and does carVistior.visit(this)

CarVisitor interface one method visit()

Concrete Booster2x implements Car, where visit() method doubles the car speed

Same with Booster3x where visit triples car speed

> Q: whats the difference between this vs. decorator? why not have booster as a decorator?

> Decorator pattern has a bigger intention to not explode the number of subclasses. We could definitely use decorator for this case, but they solve different issues... Although they do both help add additional functionalities to an exisiting class.

Final result will look something like this:

```java
Car myCar = new Honda()

myCar.accept(new Booster2x())
myCar.accept(new Booster3x())
```

^ I guess for decorator pattern, it will modify the object from the "outside" by creating another object in its constructor :thinking:

Everything depends, depends on what you're solving for. Do we always want a facade? Do we want a middleman? Will the positives outweigh the negative?

## Template Pattern (B)

Behavioral pattern

Problem: multiple algorithms share common structure, can be tedious and error-prone to duplicate common code in each implementation

Basic inheritance: parent contains shared code (**invariant** code) that gets implemented by child (**variant** code)

Example: Tea and Coffee, where boilWater(), pourInCup() that are invariant, but some are variant steepTeaBag(), brewCoffeeGrinds(), etc.

They control their own algorithms in `prepareRecipe()` (this is a good thing)

However, there's no inheritance involved, meaning:
- They do not have a shared parent, there is no contract that they have to boil water, pour in cup, etc.
- hard to enforce structure
- changes have to be made in multiple places

Template pattern coming in:
- Abstract class contains templateMethod() that is **final** and cannot be modify, tells beverage you must do step 1, 2, 3, 4, etc.
- Template method controls the sequence, aka the algorithm skeleton
- ends with hook() method, which is optional (but very important). Allows something to happen at any point in time...
  - this is similar to visitor pattern where injection is allowed
  - children would work even if they don't implement the hook!

So the abstract class for Coffee and Tea is now:

```java
public abstract class Beverage {
    final void prepareRecipe() {
        boilWater();
        brew();
        pourInCup();
        if (customerWantsCondiments()) {
            addCondiments();
        };
    }
    // Contract for functions above... concrete or primitive or hook...
} 
```

Participants: abstract class, concrete class, and template method

Parent class owns algorithm, subclasses fill in the blanks

> How is this different from regular inheritance?

"Don't call us, we'll call you"

Usually, subclass calls parent method, but here parent class calls the subclasses

Summary: Template design pattern is to enforce a common structure in similar algorithms or procedures.
