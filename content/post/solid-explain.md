---
title: "SOLID - Briefly Explained"
date: 2023-08-08T10:17:48+07:00
subtitle: What is SOLID, anyway ?
tags: ["software-development", "blog", "SOLID", "OOP", "EN"]
draft: true
---

<!--more-->

## Quick Overview

At first, this blog was gonna be about Dependency Injection to clear out the confusion since it sounds more complex than its idea and application. However, as I make preparation for the blog, I realized I should talk about IoC design pattern (Inversion of Control) as DI is a sub-type of IoC and the idea of IoC implememtation originated from Dependency Inversion Priciple, 1 of the priciple in SOLID, so here we are. I will make this blog explain briefly the idea of SOLID and each principle will have a more in-depth blog with examples.

Let's get start with the basic. If you have heard about Object-Oriented Programming or OOP, you should know the 4 pillar of OOP, which are: 
* **Abstraction**: Hiding unnecessary detail implementaiton of an object's internal structure; Keeping its structure and behaviors separated to make it easily understandable.
* **Encapsulation**: Bundle the data and related functions of said data together into a single unit which is a object; Limit access to the data to prevent over-reliance and misuse of object's data; Ensuring their proper functioning.
* **Inheritance**: Create a new class (child class) the inheriting the existing class (parent class), the child class typically has attributes from the parent class (includes data and methods), thouh the child class can redefine those attributes as well.
* **Polymorphism**: Allow using interchangeably objects of different classes, as long as they implement a certain interface (have methods of the same name); Promotes flexibility and dynamic behavior based on the actual object type, achieved through method overriding and interfaces.

These concepts are commonly known and have become staple in technical interview where it come to OOP. However, you will also hear of SOLID principles which utilizes these OOP attributes and help developers create code that is easier to understand, modify, and extend.

## What is SOLID ?

SOLID is an acronym that represents 5 principles of object-oriented software design and programming. These principles were introduced by Robert C. Martin (also known as Uncle Bob) and are considered fundamental guidelines for writing maintainable, scalable, and flexible code. 5 principle are the following:

1. Single Responsibility Principle (SRP)
2. Open/Closed Principle (OCP)
3. Liskov Substitution Principle (LSP)
4. Interface Segregation Principle (ISP)
5. Dependency Inversion Principle (DIP)

## Single Responsibility Principle (SRP)

> "A module should be responsible to one, and only one, actor."

A module a part of the program - a class, a function or a object - should only be delegated one reponsibility, and should only be changed for one reason: improve on handling that responsibilty. Let's look at an example that doesn't follow this principle

```
class ReservationManager:
  def GetReservationFromDB(self, data):
  def ProcessReservation(self, data):
  def WriteReport(self, data):
```

This ReservationManager class currently has 3 reponsibility: Get reservations from Database, Process reservations, and Write reports. If there is any change occur in these 3 tasks (maybe change in the database, priority change in reservation processing or format of the report), we need to change it in this class, result in making this class more bloated.

Adhering to Single Responsibility Principle, we should split this class into 3 smaller class for each funtionality for better clarity. Even though there are more classes than before, the responsibilty of each class is more easily understandable for us and other collaborators, both in the case of development and debugging.

## Open/Closed Principle (OCP)

> "Software entities (classes, modules, functions, etc.) should be open for extension, but closed for modification"

Similar to we don't need to open the engine of a car to change its tire, once a software component is designed and implemented, we should avoid altering it to add new features or behaviors. Instead, new functionalities should be achieved by creating new classes or modules that extend the existing ones.

This approach promotes code reusability, reducing the risk of introducing unintended side effects when modifying existing code, and enhances the overall stability of the software system. Developers can easily extend the capabilities of their software while maintaining the integrity of the existing codebase.

## Liskov Substitution Principle (LSP)

## Interface Segregation Principle (ISP)

## Dependency Inversion Principle (DIP)


