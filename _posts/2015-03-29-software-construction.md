---
title: Software construction (implementation) in Software Engineering
author: Ivan Penchev
date: 2015-03-29 12:30:21 +0100
categories: [Programming, Software Engineering]
tags: [Software Engineering]
toc: true
---
# Chapter 6

## Introduction

In this chapter we will turn our attention to creating working software, this is the responsibility of the “Construction” Engineering activity. While writing code [ coding ] is the core of Construction, there are a set of concepts and principle needed for efficient execution, those aspects are going to be covered, briefly, now.

## Translating Design into Construction

A definition of Construction would be “Transforming design into an executable program”. Practically the relationship between Design and Construction activities varies based on the selected process model. For example in the traditional (Waterfall) model, Construction occurs only, when Design is completed. In that model, construction refers practically to coding.

In Iterative/RUP process model, however, Construction refers to a mix of requirements, design, coding and testing activities.  Figure 1, gives us a visual presentation of that.

![]({{ "/assets/img/software-engineering/Ch06_IMG_01.jpg" | relative_url }})

Figure 1 Construction Iterative/RUP

Similarly in Scrum, for example, Analysis, Design, Coding and Testing happen in multiple sprints. And unlike RUP, there is no clear separation between these activities. The scope of Construction is decided by the scope of iteration work (e.g. n iteration, where n is a big number = a shallow scope of Construction, meaning during to the big number we could allow ourselves to do multiple, but small (shallow) piece of Construction)

**So what do we mean by Construction?**

è _Construction: Specifically coding and its related principles__._

We will discuss the activity, regardless of the process model. Because the Principles of Software Engineering activities are the same. And each process model, then brings it’s own flow and emphasis of activities.

## Principles of coding

“Any fool can write code that a computer can understand. Good programmers write code that humans can understand “

—Martin Fowler

Being a clever, should never be an aim for a programmer. They shouldn’t also aim for a fancy code for achieving functionality. Instead they should always aim for simple and readable code.

Simple code is easy to maintain, review, evolve, test, and document. There are various standards for writing simple code, such as: 1) Following standards 2) Following design best practices.

Code is simplified by making use of coding standards. Such as naming, layout and commenting conventions. There are various sources for standards: 1) can be adopted from a specific platform (e.g. C# coding standards) or 2) from External organization (e.g. modeling using UML), and of course 3) could be built internally.

Regardless if using eternal or external standards the important thing is to apply them consistently. Of course adopting external standards, make it possible for employees to be aware of those standards beforehand.

As an example, a link showing C# coding conventions [http://msdn.microsoft.com/en-us/library/ff926074.aspx ].

Following design best practices during coding is essential.  They come in different “flavors”, but we will categorize them in 3 cases: 1) General practices (e.g. modular design and decomposition), 2) Programming methodology – specific practices (e.g. SOLID of OO Programming) and 3) platform/language -  specific idioms and practices (e.g. .NET design guidelines for exception handling).

Language idioms capture solutions to language-specific problems. Think of them as the equivalent to architectural styles (at architecture level) and design patterns (at detailed design level) at the coding level.

Change is inevitable for software. It can be due to environment or due to change in requirements. Therefor building _extensible_ code is a key aspect of Construction.

This means that Code adapts to changes with minimum impact on overall structure and behavior.

**Some techniques for achieving such code, are:**

*         High cohesion

*         Low coupling

*         Abstraction and encapsulation

*         Configuration files and dynamic rules

*         Decouple code for environment and infrastructure

*         Language/platform specific techniques – such as C# Attributes and Reflection

Reuse of code, enables productivity, cost saving and increased quality. **Reusable code** refers both to writing such code, and reuse of code from other sources.

_Writing reusable_ code mean Writing components that are independent from a specific system or

Subsystem (e.g. Logging and exception handling components).

_Reusing other’s_ code, means utilizing community or 3<sup>rd</sup> party components.

## Testing

Testing is a Software Engineering activity, for which we are going to talk in the next chapter. However it is critical, to not leave testing entirely till after construction. If this happens the cost of finding and fixing problems, becomes huge.

Instead developers, must implement a subset of test types during development. This reduces the gap between the time faults are inserted into the code and the time they are detected and corrected.

**Two testing types must be part of Construction:**

*         Unit Testing

*         (Limited scope) Integration Testing

Unit testing is so entangled with coding, that now is considered more of a coding practice than a testing practice.

Unit Testing:

1. Write a function or other block of code
2. Create a unit test that verifies:

Behavior of code in response to standard, boundary, and incorrect input data

Explicit and implicit code assumptions

In other words with unit testing we do not wait until we have an executable version of our program, instead the testing occurs on the code-block level.

So how do we test non(un) – executable code?

We typically need a “stub” and a “driver”. The last simulates a calling unit, and the first simulates the called unit.

**Test Driven Development (TDD)** development practice even advocate to:

Create unit test before writing the code to be tested

In TDD Unit tests also serve as design documentation and functional specification

Integration testing is about bringing together individual components and testing system or subsystem functionality in terms of these interacting components.

The scope of the integration test, during Construction is limited to a certain functionality that shouldn’t be left untested till the Validation phase. For example: Component A decrypts a message && Component B transforms the output of component A à Testing the integrating between A and B is the only way to guarantee that both components perform their intended functional scope.

However integrating entire subsystem in a simulated real-time environment, happens in the Testing phase.