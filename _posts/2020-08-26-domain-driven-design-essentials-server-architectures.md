---
title: Domain-driven design essentials - Server Architectures
author: Ivan Penchev
date: 2020-08-26 11:53:21 +0100
categories: [Programming, Software Architectures]
tags: [Domain-driven design, DDD, Software Architectures]
toc: true
---

# Chapter 1
## Introduction
In the last 10-20 years, a huge hype has accumulated around microservices and domain-driven design (DDD), and rightfully so. I am writing this body of articles as a way for me to lay down my exploration knowledge I have accrued on the topics and share it with other curious people.
I will be focusing specifically on DDD, but we will also briefly talk about Mircroservices architectural style and why DDD and microservices compliment each other so well. To illustrate core topics two projects are provided, source code for which is located [here]({{ "/assets/src/ddd_example_projects.zip" | relative_url }}), a project about building a blog and a project about building a car rental service.
### Resources
A huge inspiration for me has been the work of Eric Evans, the so called ["Blue book"](https://www.amazon.com/exec/obidos/ASIN/0321125215/domainlanguag-20), Jimmy Boggard, Julie Lerman, and to a degree Martin Fowler.
My advice for you is to check them out post reading this body of work, as their creation is not necessarily targeted at introductory level. The latter statement is especially valid for the Blue Book, even though I find the [non technical path](https://domainlanguage.com/ddd/nontechnical-path-through-the-book/attachment/pdf/) as a bit easier.