---
title: Domain-driven design essentials - Key Concepts Continued
author: Ivan Penchev
date: 2020-09-05 08:22:21 +0100
categories: [Programming, Software Architectures]
tags: [Domain-driven design, ASP.NET Core Microservices, DDD, Software Architectures]
toc: true
---

# Chapter 2
## The Domain Model - Continued
![contexts](({{ "/assets/img/soft-architectures/ddd/01/ddd-diagram-objects.png" | relative_url }})

Fig. 05 DDD diagram for other Domain objects

We are continuing our journey in the Domain model.

### Aggregates

As we discussed the domain model’s is composed of entities, enumerations - possibly, and value objects that represent concepts in the problem domain. Hands down the biggest problem when transferring the problem domain to a domain model in code , is the number and type of relationships between domain objects. 

An example of the complexity of this issue are relationships that do not support behavior and exist only to reflect reality, another example are relationships that can be traversed in more than one direction. This defines the equal importance of not only designing the domain objects themselves but also designing their relationships.

So we are using aggregates to group related objects together, however their is also a rule that there is a single point of entry of that group of domain objects. This can define an aggregate as a cluster objects which have different types, that work together and are treated as a single unit. This cluster has a root, which is the only object with which communication is done, that is the only connection to the other "world" (layer).

Now let's look at our Car Dealers demo, if we check the Car Dealers\Domain\Models we would see that we have Car Ads models and Dealers models, let's further examine the first.

```
├───CarAds
│       CarAd.cs 			# Entity && Aggragate root
│       Category.cs 		# Entity
│       Manufacturer.cs 	# Entity
│       Options.cs 			# Value Object
│       TransmissionType.cs # Enumaration
```

Now from business perspective there is not need to have Manufacturers by itself  without Car Ads, or no point of having Categories by itself without Car Ads. We can see that our primary business unit is Car Ads, this means that all of the other entities bellow are connected to the Car Ads.  Some of you will point out that is self explanatory that when we have a Car Dealers website our primary business unit is Car Ads, and indeed it is like this, however if we look from purely structural concept isn't it much easier to say *we have Categories and we put Car Ads in them*? It is, but from business perspective you can make a website for Car Ads where we have the Ads but we don't have Categories.

But are we using this "root" only because of the business? The answer is not only. This restricts the developers when creating/using models, additionally if we scale the project, let's say we have 500 models,  instead of having 500 ways of creating a model we now only have 100. Or in other words the complexity of 500 classes communicating with each other suddenly drops significantly to 100.

So aggregate roots are cluster of objects that are saved & retrieved a single unit. The root should maintain self-integrity & validity, as it is responsible for sub entities/objects. In other words the root works as a transactional boundary, which the developer should not try to skip over. In general is a good practice to make the roots serializable.

Now let's return to our Domain Model that we were exploring in Chapter 1 - the veterinary clinic. 

> We had the following design:
>
> * **Appointment** is the aggregate root
>
> * **Client**, **ExamRoom**, **Patient**, and **Doctor** are aggregates below it

But after discussing with the domain experts not there is this invariant in our business rules - two Appointments should not overlap one another. So we change the design.

> * The aggregate root becomes a new entity called **Schedule**
>   * It contains a collection of appointments
>
> * Every appointment logic goes through the **Schedule** root
>   * It validates the state of the application is valid

### Factories & Repositories

Factories & Repositories are two concept in DDD that we can implement in multiple ways - method, classes, helper classes and so forth. The idea is that Factories & Repositories have a specific use case in DDD and it's up to us to decide how to implement them.

#### Factories

Business domain objects often become too complex, and a huge thing to remember about those complex domain objects is that they **<u>cannot</u>** be in an invalid state. Due to the aim to always be in a valid state constructors for the complex domain objects tend to become quite large and difficult to use.

If we look for our simple and base system Car Deals, we can see this:

```CSharp
C:\Car Dealers\Domain\Models\CarAds\CarAd.cs
...
16         internal CarAd(
17             Manufacturer manufacturer,
18             string model,
19             Category category,
20             string imageUrl,
21             decimal pricePerDay,
22             Options options,
23             bool isAvailable)
24         {
...
```

We have seven properties for something as simple as a demo system. This is too much!

In some cases a static factory method may be good enough, a small note however is that having a method that takes seven parameters or a constructor that takes seven parameters is pretty much the same. So as our object complexity increases we can introduce the so called Builder Factories, or we can use Fluent API, which to build our objects.

So for a  bigger solutions introducing a build object is recommended. To do that we mark constructors as internal, we mark all aggregate roots with an interface and we create a factory for each aggregate root.

##### Anti-corruption Layer (ACL)

This is a very interesting concept written in the "blue book" by Eric Evans. 

> *“When systems based on different models are combined, the need for the new system to adapt to the semantics of the other system can lead to a corruption of the new system’s own”*
> -Eric Evans

Even thought the main underlying concept is pretty clear. When we need to “talk” with **other** system, data repository or legacy code ( even “ours” legacy code ) we should protect our model from other “foreign models”. We could use this concept even on the factory level. We do this by creating an interface that only builds factories for Aggregate roots, and since all objects have internal constructors, and we can only build via the factories, we technically make ACL for what object we are able to instantiate in our system.

```csharp
C:\Car Dealers\Domain\Factories\IFactory.cs
1 namespace CarRentalSystem.Domain.Factories
2 {
3     using Common;
4
5     public interface IFactory<out TEntity>
6         where TEntity : IAggregateRoot
7     {
8         TEntity Build();
9     }
10 }
```

Then we use this interface to create a concrete factory implementations. We can see an example in \Domain\Factories\Dealers\DealerFactory.cs .

We talked about ACL very lightly, you can read more about it:

* [Domain Driven Design Distilled by Vaughn Vernon](https://www.amazon.com/Domain-Driven-Design-Distilled-Vaughn-Vernon/dp/0134434420) 
* [Anti-Corruption Layer pattern by Microsoft (this is on a system level)](https://docs.microsoft.com/en-us/azure/architecture/patterns/anti-corruption-layer)
* [Four strategies for dealing with legacy systems by Eric Evans](https://dddcommunity.org/library/evans_2011_2/2)

#### Repositories

Repositories are the main pop-up keyword that you will find when you do some research for ACL. 

> **NB: When we talk about *<u>Repositories</u> *normally people think database; <u>DDD Repositories != EF Repositories</u> **

Repositories are connected to the business logic inside concrete domains. For example in our system that would be Find all Car Ads by this Category. In other words Repositories work for operations on the domains, and we should only use them on the Aggregate roots. 

We can split the  repositories into:

* Domain only repositories – work only with domain models
* Query repositories - map domain models to DTOs

Regardless of the type our aim is to have specific repositories, and not just one generic one, with the specific business requirements - because all this speak the business language. So here comes the important distinguishment for repositories in DDD, compared to Entity Framework, we do not think in terms of persistence, but rather we take it as business logic for communication with the domain (the fact that we can have a backing store - database, behind is completely fine, but is not the main reason for existence of the component).

We implement Repositories similar to Factories using a generic interface, that limits us to only using Aggregate roots, and concrete implementations.



