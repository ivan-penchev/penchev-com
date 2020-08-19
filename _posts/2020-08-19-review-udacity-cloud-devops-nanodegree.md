---
title: A Review of Udacity's Cloud DevOps Engineer nanodegree
author: Ivan Penchev
date: 2020-08-19 17:06:21 +0100
categories: [Programming]
tags: [MOOC]
toc: true
---



## Introduction

You should be considering this nanodegree if you have absolutely zero knowledge about AWS and cloud computing in general. Some of the content covers subjects that you are very likely to know or even be very good at if you’ve worked in the field for a while – such as CI/CD, monitoring and logging.

If you are a beginner in these areas, though, you might benefit from learning what this nanodegree has to teach you. Just keep in mind that you will still have a learn to learn by yourself, as the material doesn’t go very in depth about most subjects – some of it is even treated very superficially, and feels a bit rushed.

## Time needed for completing

Udacity estimates 4 months to complete the nanodegree, at a pace of 10 hours/week. I finished it in about 2 months and half months, still having 2 breaks larger than a week. So you might be able to save some money if you’re putting in even 10 hours/week. 

If you’re pressured by time, you can check the course projects before beginning a specific course – some of them don’t actually require you to go through all the course material, so you can save some time this way. You can always come back to those materials after finishing the nanodegree, as they’ll still be available.

### 

## Content

The main 4 courses, that this nanodegree is divided into are as follow: **Cloud Fundamentals**, **Infrastructure as Code**, **CI/CD** and **Microservices at scale**. Each of them ends with a project that is reviewed by Udacity, and that need to be passed in order for you to graduate. Most lessons have videos that are easy to follow along, so you can practice before doing the final project for each course. There is one additional course **Networking** this is extremely needed if you understand what VPC is and what are the building blocks of it.

### Cloud Fundamentals

> In this class you will be introduced to compute power, security, storage, networking, messaging, and management services in the cloud. While learning the fundamentals, you will explore tools and services offered by Amazon Web Services (AWS) through interactive hands-on exercises. 

As you might expect, this course covers the basics of AWS and lays the foundation for the rest of the nanodegree. Not all the topics covered here are needed in the other courses, but it’s nice to have an overview of what is achievable with AWS.

The concepts are nicely explained and, if you actually have no prior experience with AWS, you can start experimenting with the different services and see how they work, or how they can be integrated.

The final project is pretty straight forward: deploying a static website on Amazon S3. Where this is more like a tutorial with steps, as the creators are trying to guide you into the world of cloud computing.

### Deploy Infrastructure as Code (IAC)

The idea of IAC is pretty straight forward

> *Infrastructure as code (IaC) means to manage your IT infrastructure using configuration files.*

If my application needs one server, instead of my manually creating that server I can write a configuration file, that creates the server. This way I know that:

1. I can reuse the configuration file to create more servers
2. Every new created server will be exactly the same

In this class you will cover CloudFormation (an AWS service) to deploy your infrastructure through code, as well as designing diagrams for your infrastructure. 

The final project expects you to basically combine the knowledge from the course to deploy a simple application to AWS – even if you are stuck, there are  AGAIN guidelines that are easy to follow and direct you to the desired result.

### Building CI/CD pipelines, Monitoring & Logging

This is probably not only the worst course from this nanodegree, but probably the worst course I have ever attended.  It tries to teach CI/CD with Jenkins (with the Blue Ocean plugin), Ansible, and monitoring with Prometheus.  However things are very sporadically made, with almost no connection between the different pieces.

The final project is deploying a Jenkins instance on EC2 and running a pipeline on it.  This is probably the only course that I would advice you to skip the content of and try to do the final project using different tutorials that you find online. Not only is the content not worth it, but searching and finding things by yourself would help you much more .

### Microservices at Scale using AWS & Kubernetes

This one is quite possibly the most interesting course, as it touches the basics of Docker and Kubernetes. It teaches you how to create the container and run it locally, as well as how to configure a cluster locally with minikube. The name of the course seems a bit misleading, as it doesn’t actually teach you how to deploy the containers to any AWS service. It does for some reason speak briefly about Cloud9 (an cloud based IDE) and AWS Lamdba (an AWS serverless backend), but it feels like they are just there so that you know they exist rather than for some coherent use during the project.

It’s a good base if you have no prior knowledge on these subjects, but there’s a lot more research you will need to do if you want to be able to finish the capstone project.

The course is better than the CI/CD one, but it could definitely benefit from a bit more work and refinement.

### Capstone project

As expect the final project wants you to combine everything you have learned so far with a small twist, figuring out how to deploy an Kubernetes cluster on AWS (EKS) using IoC, that you will use inside of your Jenkins pipeline to deploy.

I personally feel like I learned more by developing the capstone project than by going through the rest of the lessons. Especially with some of the issues that I got. For example in order to create an EKS an role for the cluster must be assigned, if the user that you are using to access the cluster does not have that role he won't have permissions. If the node group of the EC2 instances connected to the cluster, don't have that role it will be not be accessible. 

it will take a few days of struggling but it's quite doable. Also you can use the GIT repos of everyone that has graduated, to look for inspiration.

## Conclusion

Is this nanodegree worth it? If you are paying for it - NO. it is simply too sporadic for an absolute beginner and too shallow for more experienced person. Also the price tag is pretty steep for what it provides as content. 

If you are a beginner looking for start in DevOps, I would advice you to look somewhere else. For example https://kodekloud.com/p/learning-path [I am not sponsored by Kode], as the price is better and all the content goes trough a single huge project, rather than multiple mini ones.