---
title: A Review of Coursera's / University of California's Programming with Google Go Specialization
author: Ivan Penchev
date: 2021-11-16 08:22:21 +0100
categories: [Programming, Go]
tags: [Golang, Go, Reviews]
toc: true
---

## Introduction

You should be considering this specialization if you have absolutely zero knowledge about Go, but not zero knowledge about general programming concepts in general. Some of the content covers subjects that you are very likely to know or even be very good at if you’ve worked in the field for a while.

If you are a beginner in these areas, though, you might benefit from learning what this specialization has to teach you. Just keep in mind that you will still have a learn to learn by yourself, as the material doesn’t go very in depth about most subjects – some of it is even treated very superficially, and feels a bit rushed.

## Time needed for completing

Coursera / University of California estimates 3 months to complete the specialization, at a pace of 3 hours/week. I finished it in about 1.5 months and half months, still having 2 breaks larger than a week. So you might be able to save some money if you’re putting in even 3 hours/week. 

If you’re pressured by time, you can check the course projects before beginning a specific course – some of them don’t actually require you to go through all the course material, so you can save some time this way. 

## Content

The main 3 courses, that this specialization is divided into are as follow: **Getting Started with Go**, **Functions, Methods, and Interfaces in Go**, and **Concurrency in Go**. Each of them ends with a project, that is peer reviewed, that needs to be passed in order for you to graduate. Most lessons have videos that are easy to follow along, so you can practice before doing the final project for each course.

### Getting Started with Go

> Learn the basics of Go, an open source programming language originally developed by a team at Google and enhanced by many contributors from the open source community. This course is designed for individuals with previous programming experience using such languages as C, Python, or Java, and covers the fundamental elements of Go. Topics include data types, protocols, formats, and writing code that incorporates RFCs and JSON. 

Honestly one of the of my fellow students have summed the course very well:

> I like the instructor, and the material was certainly worthwhile, but I do have some constructive criticism. 1. There were too many typos in the lecture slides. Spending my time debugging the professor's code strikes me as counterproductive. 2. There are a lot of things you have to look up on your own in order to complete the assignments. This isn't necessarily a bad thing, but the instructor could be clear about this at the outset. 3. The whole peer-grading scheme is ridiculous. If I went to my local university, took a course, and the professor said "you all have to grade each others' assignments," I'd be righteously upset about this. I don't see why the Coursera platform is inherently that much different so as to justify this absurd practice. Why should I trust that the other students are equipped and responsible enough to grade anything, particularly something they're learning at the same time as me?

This course is very, very basic. In the beginning the instructors says that the course is for people with knows other programming languages, but the expected knowledge is very low. For example, a very long explanation about variable scope is for absolute beginners. If you are proficient in any other language like C, C# or Java, you have listened to this explanation N-times in your life and the course is probably too slow paced for you.

So if you already know the basics, you probably don't need this course though. Not much of a deep dive, more of a "skim the surface" type course. Week 4 on IO was the most beneficial for me. The only reason why I would pay for this course is to display the certificate on my LinkedIn. All the contents given can be easily learned by doing research on your own or by visiting https://gobyexample.com/

### Functions, Methods, and Interfaces in Go

The idea of this course is pretty straight forward

> Continue your exploration of the Go programming language as you learn about functions, methods, and interfaces. Topics include the implementation of functions, function types, object-orientation in Go, methods, and class instantiation. As with the first course in this series, you’ll have an opportunity to create your own Go applications so you can practice what you’re learning.

I have mixed feelings about this course.

The lecturer is very good. He talks in a way that is inspiring and easy to follow.

But the exercises and assignment have problems. Some quizzes have errors in the available alternatives. One assignment has a severe error in the grading instructions. (It said a certain interface should be implemented in a certain way, but that way was not the way that the original instruction said we should do it). There are syntax errors in the slides.

The forum doesn't work very well. 95 % of the threads have the same topic: "Please review my code, please!!!". That could easily be avoided if it was mandatory to review more than one other student or by... automated unit testing. 

### Concurrency in Go

> Learn how to implement concurrent programming in Go. Explore the roles of channels and goroutines in implementing concurrency. Topics include writing goroutines and implementing channels for communications between goroutines. Course activities will allow you to exercise Go’s capabilities for concurrent programming by developing several example programs.

This was the supposed to be the best course in the specialization, the reason I started this whole shebang, yet what it ended being the the pinnacle that describes this whole specialization, which was very nice put by a fellow student:

> The courses talk about fundamentals and computer science stories. The entire specialization focus is less than 50% on the Go language itself. No Go mod, No libraries, No coding...
>
> For instance, The professor is about to explain a new thing and suddenly remember forgot to mention something before so he jumps to the missing point and then jumps back to continue. HARD to follow up...

We start this course by explaining why we need/do things concurrently, starting as far back as to explaining Moore's law and why we can't scale computing power resources up indefinitely. I've been coding professionally for 7 years now, and I still find the basics explained in this course to be useful and refreshing, don't get me wrong, but spending 50% of the course on them? I still learned a good amount of details about Go, but definitely we could have spent more time talking about the language intricates.  

### Capstone project

or the lack of such. Which is great if you are a working professional with experience in the field, but not so much if you are just starting.

This specialization could benefit a huge amount from this. In the end of the day you pass 3 courses, you do some small exercise and programs, but besides that nothing. You could check my GitHub profile and see the end result - https://github.com/ivan-penchev/coursera-google-go-professional-cert



## Conclusion

Is this worth it? If you are paying for it - NO. Actually overall strongly I do NOT suggest to waste your money or time on this specialization on Coursera.

It is simply too sporadic for an absolute beginner and too shallow for more experienced person. Also the price tag is steep for what it provides as content. 

With that being said, if you are like me and looking to learn some Go for work projects, checkout https://gobyexample.com/ and then move to https://gophercises.com/

If you are a beginner looking for start in Go, I would advice you to look somewhere else. However I could not really recommend any better course.

As a finale, if this specialization is improved it could have great value, as there are not a lot of combination of courses on the subject. Some of the improvements could be:

1. Automated grading of home assignments
2. Capstone project
3. Each of the courses could have a week 0, in which prof Harris explains the subject's history/origin, but then we focus on Go specifics only
4. Discussion forums, should only be used for discussion for the course's content

