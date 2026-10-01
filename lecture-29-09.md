# Lecture 29/09

Have so far covered Observer (observable + observer) design pattern and Strategy (no if-else statements) design pattern.

## Guidelines for Assignment 1

Prof uses IntelliJ for IDE -- maybe time to try it out

Theres a docblock style for this class, top of file:

```java
/**
 * Name: first last
 * Course: CS-665 Software Designs & Patterns
 * Date: MM/DD/YYYY
 * File Name: Main.java
 * Description: blah blah
 */
```

Important for intent, metadata, and maintainability.

Plus docblock for each method... This is better than a bunch of inline comment. Inline comments are pollution + code should be self-documenting.

Assignment 1 do not use design patterns, the rest use at least 1.

Unit tests! Structure should be:

(1) **Given** -- if data looks like this

(2) **When** -- function to test

(3) **Then** -- resulting assertion

In the real world, shoot for 100% coverage. For this class, write 3-5 tests. Those should focus on most important part of software.

Prof has version 17
Recommends at least version 8

Zip up assignment and submit it

Recommends lucidcharts (free with BU email) and drawio for diagramming.

Please use OOP.

Assignment 1 -- make it so it is easy to add features (not just milk + sugar, but also honey, etc.)

- doing optional tasks is good

UML for communication, no need for every single class (maybe yes still for assignment 1)

- Add UML as JPG/PNG. I should also embed it in README.md

Prof focused on JUnit tests for grading

If asking for user input, make sure to validate it. Imagine end users are evil.

Zen of Python (`import this`) mentioned LOL

```shell
>>> import this
The Zen of Python, by Tim Peters

Beautiful is better than ugly.
Explicit is better than implicit.
Simple is better than complex.
Complex is better than complicated.
Flat is better than nested.
Sparse is better than dense.
Readability counts.
Special cases aren't special enough to break the rules.
Although practicality beats purity.
Errors should never pass silently.
Unless explicitly silenced.
In the face of ambiguity, refuse the temptation to guess.
There should be one-- and preferably only one --obvious way to do it.
Although that way may not be obvious at first unless you're Dutch.
Now is better than never.
Although never is often better than *right* now.
If the implementation is hard to explain, it's a bad idea.
If the implementation is easy to explain, it may be a good idea.
Namespaces are one honking great idea -- let's do more of those!
```

Recommend submitting assignments day before, in case new class covers something important. But start early and pace.

Plagarism is checked: https://github.com/jplag/jplag (cool tool)

## Factory Method Pattern (C)

(C) - Creational patterns

(B) - behavioral patterns

(S) - structural patterns

Method is a function attached to a class. In java, technically everything is a method.

Params vs args: params are placeholders, args are what you actually pass in

**Overview**:

- Way to consistently create objects
- Allow subclasses to instantiate objects at runtime, subclass can decide what object to create
- When creation is expensive (BUT does not solve overhead for instantiation!!! Pattern solves for consistency)

When we have more things to manage, the worse off we'll be.

Factory has a Super Class, then we'll have classes who inherit:

- class A
- class B
- etc.

Factory pattern encapsulates object creation and delegates (abstracts) the responsibility to another class.

### Participants

Product: defines the interface of objects the factory method creates

Concrete Product: implements the product interface

Creator: declares factory method

Concrete Creator: overrides factory method to return instance of concrete product

### Example

PizzaStore uses SimplePizzaFactory with createPizza() method

Pizza can be Cheese, Veggie, Meat, etc. but the base methods of Pizza is prepare, bake, cut, box, etc.

Bank account example with checkings and savings, idea is to just call:

```java
BankAccount bankAccount= new accountFactory().createAccount()
```

#### The simple (and wrong) example

DO NOT USE THIS

Has interface Pizza with prepare() and cook() method

Then PepperoniPizza, CheesePizza, VeggiePizza implements Pizza and overrides those two methods

In main class method, manually have switch cases for the three types and creates these for each condition. THIS IS THE PROBLEM.

SOLUTION: Create new SimplePizzaFactory() with createPizza() method with the string type switch casses. BUT THIS IS BAD. This needs to implement an interface, this misses the point of being flexible and using an interface.

#### The method example

Create PizzaStore abstract class with createPizza() abstract method with the orderPizza() method. This is going to be the template method.

Then PepperoniPizzaStore() extends the PizzaStore() and overrides the createPizza() method. Then returns instance of created product.

### When to use

Class can't anticipate the classs of objects it must create

Class wants its subclasses to specify the objects it creates

### Misc

Remember open-close method, open for extension but closed for modificiation
