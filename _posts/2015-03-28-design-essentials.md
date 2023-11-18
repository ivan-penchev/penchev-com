---
title: Design in Software Engineering
author: ivan
date: 2015-03-28 17:02:21 +0100
categories: [Programming, Software Engineering]
tags: [Software Engineering]
toc: true
---
# Chapter 5  

## Introduction

With the completion of Requirements Engineering and generation of appropriate Requirement Models, we have a solid understanding of the Problem Domain. 
We will move our focus to Designing the solution, and we will talk about Design concepts.

![pic1]({{ "/assets/img/software-engineering/Ch05_IMG_01.jpg" | relative_url }})

Figure 1 Design in SE

## Analysis Activity vs. Design Activity

Before diving into Design, we will answer one common question, what is the difference between Analysis & Design.

The most direct and short answer is _that it is matter of focus._

![pic2]({{ "/assets/img/software-engineering/Ch05_IMG_02.jpg" | relative_url }})


Figure 2 Analysis vs. Design (in depth)

## From Requirements to Design

The previous two chapters covered Requirement Modeling as a critical step in Requirement Analysis. In that part I said that models have one other important feature – guiding the design phase.

In the design phase, models help us answer the following questions:

1.       How are objects grouped into system-level components?

2.       How are these components structured?

3.       How are the component interfaces designed?

4.       How do these components interact?

5.       How do these components satisfy the non-functional requirements?

6.       How are the inner classes of these components designed?

7.       How are the inner classes related?

8.       How do the inner classes interact?

9.       How events and states affect these inner classes?

As we can see from this list, design is subject to different abstraction level. When we talk about designing the structure of the major system components, their communication, interaction and how they satisfy the NFR, we are talking about high level **architecture design** (points 1-5), on the other hand when we talk about the detail design of each individual component, how the component constitutes are structured and how they interact, then we are talking about low level **detail design** (points 6-9).

## Design vs. Architecture

So as I mentioned software design has two levels:

1.       High-level design (architectural design or simply **architecture**)

2.       Low-level design (detailed design or simply **design**)

SO WHAT IS ARCHITECTURE?

<u>Software Engineering Institute (SEI):</u> “the structure or **structures** of the system, which comprise software **elements**, the externally visible **properties** of those elements, and the **relationships** among them”

<u>IEEE:</u> “the fundamental **organization** of a system embodied in its **components**, their **relationships** to each other, and to the environment, and the **principles** guiding its design and evolution”

Although the definitions are somewhat different, they both indicate some _common properties of Architecture_ such as:

1.       It focuses on **architecture-relevant** components

*    A component is module/part of the system that encapsulates other elements (data, classes, procedures, logic, etc…) and exposes an interface which defines its behavior.

From this we can define _Architecture-relevant components_ as those components that: 1) affect the overall structure and behavior, 2) Influence quality attributes (ex: performance, security, scalability, etc…) and 3) Influence environmental and technological constraints (i.e. non-functional requirements).

So an _Architecture_ defines the system structure in terms of this architecture-relevant components. It can also define the system behavior in terms of how this components interact with each other.

2.       Most of the non-functional requirements are taken into consideration.

3.       It (Architecture) may conform to architectural styles (AS)

*   AS are solutions to system-level organization problems

* AS provides a set of predefined component types, responsibilities, and relationships (e.g. Pipe-and-Filter and Publish and Subscribe are AS)

SO WHAT IS DESIGN?

Unlike Architecture Design deals with lower abstraction layer, and focuses it’s Work with the constituents of the architecture-relevant components or non-architecture-relevant components. So we can say that Design is concerned with classes, rather than components, and also with components that do not influence the overall structure and behavior.

Having this constituents and components identified, then again Design defines their structure and behavior.

And finally, while Architecture may conform to Architectural styles, Design may conform to Design patterns. They are solutions to detailed design problems (e.g. Singleton, Factory, Decorator and so on).

Let’s illustrate the following with the example of the Airplane Ticket Reservation System.

![pic3]({{ "/assets/img/software-engineering/Ch05_IMG_03.jpg" | relative_url }})

Figure 3 Architectural and Design

Architectural-relevant model components are those in white, those are in the scope of Architecture, while the components in grey are in the scope of the Detail Design.

## Abstraction levels

As I said working with software is done on multiple abstraction levels, everyone of which encapsulates complexity, looks at the system from different viewpoints (each viewpoint is meaningful for a certain stakeholder group).

At the very top we have the **Contextual** abstraction level. Questions in this lever are:

Why do we need the system? && What are the business objectives? -> They are typically answered in a project charter. (This falls outside the scope of Software Engineering)

The next level is **Conceptual**. Here we answer the questions of : What are the requirements of the System. So far, we have been doing most of our work on these level

Next comes the **Logical** level. Here we answer the questions of: How will the requirements be met? So at this level, we _create the logical architecture and design._  You can hear a typical question for this level:

**_Q: Can this level include technology information?_**

**_A: Mostly no, but there can be exceptions_**

e.g. A Conceptual-level resulted in constraint to use an existing middleware product (e.g. BizTalk, IBM Integration, etc…)

This imposes major design considerations. So it becomes a major part of the logical architecture and design

The last is **Physical**  level. Here we are busy with what will the solution be built?

Concerns are physical components, products, specifications, technologies, etc… This level is mostly an architecture concern but details could be left to detailed design.

## Viewpoints

Let’s recall, Analyst uses different models, at different abstraction levels, so that we can understand the problems from different perspectives.

Architects do the same, but rather than looking at the problem, they concentrate to model the solution from different perspectives.

**Views** are representations of one or more architecture aspects.

**Viewpoints** define stakeholder groups concerns, and then define patterns, templates, and principles for creating views

_A view can be seen as an instance of a viewpoint!_

_Benefits of Viewpoints and Views_

Creating viewpoints and views is an essential architectural skill:

*         By separation of concerns: through different – but related views – architects can focus on different aspects of the overall solution
*         Stakeholder management: different views tackle the concerns of multiple stakeholder groups
          *          Different stakeholder groups only see the information they care about (e.g.: Infrastructure team only cares about physical models (suitable for their skills and expertise)

*         Guidance for development: views guide design and development
          *          Having the proper guides for Designer and Developers helps them focus on specific scope rather than the entire architecture

## Quality Attributes

Quality attributes are non-functional requirements. They are properties of the system, rather than functionalities, for example Performance, Reliability, Security, Flexibility, etc…

Because Quality attributes are properties across the system, they are set to be cross-cutting, therefore they affect the design of _multiple viewpoints_.

Let’s consider Security in the context of 4+1 View Model {Please read about this}, see Figure 4.

![pic4]({{ "/assets/img/software-engineering/Ch05_IMG_04.jpg" | relative_url }})

Figure 4 Security applied to 4+1 View Model

Finally, non-functional requirements, contain not only attributes, but also constrains. In other words constraints – in addition to quality attributes – are non-functional requirements (e.g. Process, Infrastructure, technology, and environmental constraints)

How do this constraints affect the viewpoints?

* A constraint can manifest itself as a quality attribute (e.g. a governmental regulation can say all public communication must be secure via public/private key pair, if so this turns into Security)

* Process constraints(like adopting a certain developing methodology) for example are unlikely to affect viewpoints

* Technology constraints – such as adopting a specific middleware – will affect multiple viewpoints

## Architectural documentation

Just like Requirement models are documented in Requirements Specification Document, Architectural models are documented in an Architectural Description document (AD).

**AD essential sections:**

*         Viewpoints

*         Views ( shown through different models)

*         Quality attributes and how are they fulfilled through the Architecture

*         Other addition non-functional constraints and how are they fulfilled

*         Decisions documentation (i.e. logic behind taking major architectural decisions)

Detailed design is not part of the AD, it could be documented in different document, but often is written in the development tools, which speeds up implementation.
