---
title: Requirements Engineering in Software Engineering
author: ivan
date: 2015-03-24 15:55:21 +0100
categories: [Programming, Software Engineering]
tags: [Software Engineering]
toc: true
---
# Chapter 3  

## Introduction

In the previous chapter we completed an overview of the most important process models, now we will turn our attention to Software Engineering Activities. Requirements Engineering is the first activity of Software Engineering. It defines what the system will do, and preparation to the design activity. In this chapter we will talk about the various tasks of Requirements Engineering activity.

## System- vs. User- Requirements

Before explaining going in depth with Requirements activity, let’s differentiate between System requirements and users.

In general there are two abstraction levels of requirements: 1) User requirements (UR) and 2) System requirements (SR). We are making this distinction between the two levels, simply because different stakeholders are interested in different level of requirements

UR are described in natural language, with the possibility of simple tables and diagrams. The audience of UR are not interested in advanced modeling, instead they like a document that shows the services that the system will provide. Example of people that are interesting in UR, typically those are high-level business stakeholders and managers.

SR define exactly what will be implemented in the system. The SR document contains the models that describe the system’s services, use-cases, behavior, constraints, business processes and so on. The document forms a contract between the developing team and the customer. In addition the document represents critical input to design, development and testing activities.

In conclusion we can say that SR comparing to UR, shows more details typically in a formal modeling language.

## Nonfunctional and Functional Requirements

Requirements fall into two categories – functional and nonfunctional.

![Functional and Nonfunctional requirements]({{ "/assets/img/software-engineering/Ch03_IMG_01.png" | relative_url }})

Figure 1 Functional and Nonfunctional requirements

Quantifying nonfunctional requirements is something that the customer never does! A customer will not say “the response time for a search query under peak load of 10,000 users must not exceed 2s”, instead we will hear “the system should be fast when number of users is high”.

We must expect requirements, such as “I want a secure system” and “I want a system that is easy to use”.

In the Requirements Engineering process falls the task of transforming vague nonfunctional requirements into more quantitative form. Without being quantitative requirements a difficult to design and implement, and later measured.

In order to quantify requirements, most of the time, analyst and designers (architects) must collaborate. From here we must conclude that Analysis and Design – especially for nonfunctional requirements (NFR) – are highly **related** and **iterative.**

The process of quantifying NFR is illustrated in Figure 2.

![Quantifying NFR]({{ "/assets/img/software-engineering/Ch03_IMG_02.jpg" | relative_url }})

Figure 2 Quantifying NFR

A final note we need to make is that Functional requirements can be derived from nonfunctional such. For example, Security is a NFR, a statement such as “the system must be secured against unauthorized access” will require user login and user registration, both of which are functional requirements.

## The Process of Requirements Engineering

It is time to dive into the process of Requirements Engineering after establishing some basic concepts, such as requirements.

The first step in the process is **Requirements elicitation** in which requirements are collected from various sources. The second step is **Requirements analysis** in which we analyze and understand requirements. In the **Requirements specification** step we model requirements through various notation and documented in a system’s requirements document. And finally **Requirements** are **validated.** Of course the Requirements Engineering is an iterative process. Throughout the entire process requirements are managed via the **Requirements Management** process.

Now we will see each step of the process in details.

## Requirements Elicitation

This step is about information discovery and collection, this is because in most cases the client does not have a list of requirements, rather it is the job of the analyst to take all sort of information and translate it into requirements in the Requirement analysis step. It is important that the analyst has knowledge of the problem domain.

In order that we are able to gather sufficient information, stakeholders must be identifiable. This is part of Stakeholders Management process, which is part of Project Management umbrella activity. However since the analyst is a domain expert, in almost all the cases he will help with identifying stakeholders. Requirements elicitation cannot commence before relevant stakeholders are identified.

Once we identify the stakeholders, there are various sources of information: interview and user discussion (if the system is for mass users this can be challenging), exciting documents, marketing surveys and user questionnaires, ethnographic study and facilitated meetings, where a group of stakeholders brainstorm and refine requirements.

## Requirements Analysis

In this step customer and user needs are deeply understood. The information, gathered in the elicitation step is used to: 1) organize requirements, 2) identify ambiguous, conflicting and inconsistent requirements, 3) Identify relationships between requirements, 4) Ranking requirements priority based on user needs.

In the end of this step we must have requirements that are precise enough to be validated, their cost estimated and their implementation later to be verified.

Requirements must be:

* Complete (e.g. when setting lower limit always set upper limit and out of range scenario);
*  Consistent (no confliction between requirements);
*  Without implementation details (say “what” function is required, not “how” to do it);
* Unambiguous (Can only be interpreted in a one and only way);
* Traceable (is must have a unique ID/number – so that we can link it with models);
* Written in a quantitative manner (especially for NFR);
* Achievable ( e.g. have the same lag in the HQ and other offices for the system – not feasible);
* Testable [Verifiable] (e.g. “the system must be user friendly” is not testable).

Questions such as “How to achieve the above mentioned attributes” and “what notations to use when interacting with different stakeholder groups”, are tackled from Modeling, in the next point we will briefly describe the importance of such step.

## Requirements Specification and the Importance of Modeling

In order to guarantee effective Requirement Analysis i.e. achieving attributes, we have two important conditions. These two conditioned are visualized in Figure 3.

![Conditions for effectively gathering and analyzing of requirements]({{ "/assets/img/software-engineering/Ch03_IMG_03.jpg" | relative_url }})

Figure 3 Conditions for effectively gathering and analyzing of requirements

Modeling helps the analysis achieve these conditions! As such we could answer why modeling is important, that is because it describes the system from different abstraction levels. For example if an analyst is trying to understand the system’s behavior towards an outside event, he will use behavioral models. Since the models are focused on a specific aspect of the system they will not only help the analyst understand and refine the way the system should work, but will also serve as a communication tool with the stakeholders interested specifically in the behavioral aspect of the system.

One of the features making Modeling important is that the models we create are the basis of the Design activity and the creation of design models. Other cool feature of models is that they could be used to design Acceptance test.

Now continuing to **Requirement Specification** step, this is about the production review, evaluation, and approval of the System Requirements Specification document. This document contains both the User and System Requirements. Clearly Requirements Specification step is closely related to Requirements Analysis.

A typical outline for the System Requirements Specification Document (SRS Doc) will include:

* Introduction: Purpose, Scope, Definitions, Acronyms, and References
* Overall Description: User Requirements, Constraints, and Assumptions
* Models: Viewpoints (Context, Data, Functional, Behavioral, etc…)
* Validation Criteria: Test Cases that will verify requirements
* References: Relevant documents (elicitation, vendor specific, standards, etc…)
* Appendix

You can read more about how to write SRS document here: [http://techwhirl.com/writing-software-requirements-specifications/ ]

## Requirements Validation

This step is about finding potential problems with the requirements. Finding any problems now, reduces the cost of fixing a mistake that was found later in the lifecycle. Requirements Validation overlaps with Requirements Specification to verify requirements are complete, consistent, unambiguous, verifiable, achievable, and implementation-free.

Various techniques could be used for Requirements Validation, such as 1) Formal Reviews (analyst and stakeholder review the specification document), 2) Prototyping (making customers and user segment representatives to try out an executable model – while experimenting they can confirm or not if the specifications need to be changed/modified) and 3) Test Cases (Designing Acceptance Test Cases reveals problems in the requirements {ambiguity, inconsistency, verifiability, etc.} ).

## Requirements Management

Requirements Management Relates to Software Configuration Management and Change Management. It is the process of controlling changes to requirements.

Requirement Management is concerned with various aspects of that, such as:

* Change Control process: impact and cost assessment from changes of requirements

Requirement Tracing is essential to the Change Control process, because knowing:

o   Requesting stakeholder (backward tracing)

o   Relation to other requirements (dependency relationship)

o   Linkage to design artifacts (forward tracing)

Then the impact and cost of the change can be assess:

* Version Control: document and artifacts approved versions
*  Status Tracking: informing users of status change (proposed, approved,
* rejected, implemented, verified, deleted)

## What is next?

Modeling is a core activity that ensures effective Requirement Analysis, there are multiple methods used for this activity, and that is what we will examine in the next chapters.

![]({{ "/assets/img/software-engineering/Ch03_IMG_04.jpg" | relative_url }})

Figure 4 in the Next Chapters
