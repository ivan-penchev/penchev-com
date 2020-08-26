---
title: Domain-driven design essentials - Server Architectures
author: Ivan Penchev
date: 2020-08-26 11:53:21 +0100
categories: [Programming, Software Architectures]
tags: [Domain-driven design, ASP.NET Core Mircroservices, DDD, Software Architectures]
toc: true
---

# Chapter 1
## Introduction
In the last 10-20 years, a huge hype has accumulated around microservices and domain-driven design (DDD), and rightfully so. I am writing this body of articles as a way for me to lay down my exploration knowledge I have accrued on the topics and share it with other curious people.
I will be focusing specifically on DDD, but we will also briefly talk about Mircroservices architectural style and why DDD and microservices compliment each other so well. To illustrate core topics two projects are provided, source code for which is located [here]({{ "/assets/src/ddd_example_projects.zip" | relative_url }}), a project about building a blog (simplistic project) and a project about building a car rental business (advance project).
### Resources
A huge inspiration for me has been the work of Eric Evans, the so called ["Blue book"](https://www.amazon.com/exec/obidos/ASIN/0321125215/domainlanguag-20), Jimmy Boggard, Julie Lerman, and to a degree Martin Fowler.
My advice for you is to check them out post reading this body of work, as their creation is not necessarily targeted at introductory level. The latter statement is especially valid for the Blue Book, even though I find the [non technical path](https://domainlanguage.com/ddd/nontechnical-path-through-the-book/attachment/pdf/) as a bit easier.

## What are we solving with DDD and why DDD?
Software in the last 20 years has evolved a lot. Software is everywhere. Every business has software, every business is trying to automate some processes so it can save on costs. We are literally in a "software revolution" lead by the business. 

This war on evolving your business, and it's software with it, creates a new level, a level which makes the softwares more heavy, more bloated, a level that brings more business complexity in the software domain. This makes implementing software solutions harder than it was 10 years ago. I was not a software engineer 10 years ago, so this is not a first hand account of the events, but by reading different books and articles you can see the picture.

Let's start talking tech, if we take one .NET Core MVC, or Unity project, WPF project whatever... each one of these technilogies gives us a "front-end" pattern, a well developed and a well structured one, they are great (but!). 
MVC for example has Models, Views, Controllers, everything is separated and it has it's own responsibilities - perfect. However if we have complex business logic, and as I mention the business has started bringing more complexity, even a simple application can become a plate of "spahetti" in no time. 
So if we are just using the classical pattern via MVC, we will soon have Controllers that are too long and contain a lot of business logic. This in itself does not sound too bad, if we imagine this application is developed by 5 developers and 2 QAs for a company that has 50 employees, however if we take this example and try to apply it to a corporate enviourment, this classical pattern is just not enough.
The standard approach to solve this problem is to move the logic from the Controllers into the Service layer, via creating multiple services. And this is not wrong, it just has side effects. For example as the Service layer increases different services start talking between each other, we have servies that have pure business logic, and some that contain infrastructure logic, but often we have a mix of logic inside a single service. 

What DDD is trying to do is separete the responsibilities, creating new objects, persisting in the DB and just pure business logic. The latter are things that do not dependend on anything else, those are things that I can write in plain C# code, without even installing one nuget packet. In other words DDD is trying to not only separe our infrastructe logic, persistent logic (as we are already used to) and so forth, but take the business logic and split it and cathegorize it into  different elements, this makes the code cleaner and easier to work with.

## Server side architectures
> **DDD is not only usable on the server, however our focus will lay in this area**
### Elements of an architecture