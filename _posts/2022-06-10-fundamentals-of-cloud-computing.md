---
title: Cloud Computing Fundamentals
author: ivan
date: 2022-06-10 08:22:21 +0100
categories: [DevOps, Cloud]
tags: [Cloud]
toc: true
---

## History of Cloud Computing

> If you know your history, then you would know where you are comming from...
>
> Bob Marley

Let's begin with a little bit of a history of cloud computing and this actually goes back quite a ways, all the way back to about 50+ years. But back in those days, in the **1960s**, we had standalone mainframes. And the idea behind the mainframe was that everything that you were doing in your environment ran on the mainframe. So your client systems that we think of today, such as a desktop computer, really didn't do anything. All they really had was a keyboard and a screen that would attach them to the mainframe. So every application, every service, absolutely everything that any person was doing in front of a terminal as it was known was actually happening on the mainframe. They had a term ***dumb terminals*** because they did not have any processing. They were just really long extension cords for a keyboard and a monitor back to the mainframe.But this did support early virtualization in those mainframes up around the **1970s**. And ***virtualization***  is the ability to run a system within another system.

![Fig 1. Mainframe layout](http://www.tikispaceshipearth.com/wp-content/uploads/2017/08/layout.jpg) 

Fig. 1 Mainframe layout

Today we talk about virtual machines. This is really the same idea. It's one single collection of hardware (a rack) that is able to run more than one installation of an operating system or an application. So despite the fact that there was this early virtualization capability, there started a progressive shift from this centralized computing, which again was everything running on the mainframe to a more client-server type of system in the 1980s. This is where we started having smaller systems because mainframes were physically **very** large. So a few more smaller servers were introduced wherein we could store data and basically access the information that we needed from those servers. But we could run the programs that we wanted to run on our desktop computers.

Now applications no longer ran on the mainframe. We were able to install software on our own desktop computer, so the ***dumb terminal*** was no more. Our systems were able to do the processing to handle the workload of running that application. It was simply the data that we needed to access that resided on the servers. So that's the ***standard client-server model*** and, which was very widespread throughout the **1980s** and into the **1990s**. 

So in the **1990s**, or also know as the begining of the internet era, we started seeing two interesting ideas:

1. Accessing services over the internet 
2. Distributed computing

The idea of ***distributed computing*** , and one of the earliest examples was ***distributed customer relation management software***, is basically an application to ran across multiple servers as opposed to just one or, of course, on a centralized single system. So the whole idea now is that the application went from centralized on the mainframe to a client-server model to distributed across multiple locations. But those <u>multiple locations were still under your control</u> in your organization. Offering it as a service means that the vendor themselves (or somebody else that is reselling it) they are running the application and you simply access it over the Internet. Now this also introduced server hardware virtualization over the Internet. Not only could we access applications, but we could actually access the services offered by various hardware, networking components, firewalls, for example. Things like that could also be accessed over the Internet.

And in the **late 1990s** accesing distributed services over the internet ultimately brought us to the rise of cloud computing. Cloud computing in and of itself is a relatively new service . But there are similarities in other, simillar, services that we've been using for a very long time, if you think about your phone service, for example, none of us as individuals have the means to create all of the infrastructure necessary to make a phone call. I can't go out and start putting telephone poles in the ground and running wires to all of the houses and configuring all of the services necessary to make a phone call. So I use the infrastructure that's already in place. I simply subscribe to the service that is already being offered. That's the same idea with cloud computing.

There's already a lot of organizations that have this necessary infrastructure to provide for me the three main subclassifications of Cloud computing: 

* Infrastructure as a Service, 
* Platform as a Service, 
* and Software as a Service. 

***Infrastructure as a Service*** typically deals with accessing more so the hardware, the compute resources that are necessary. So virtual machines themselves, storage, networking components, all of those fall under the Infrastructure as a Service component. ***Platform as a Service*** is generally used for software development.  It's not just a matter of sitting down at a single computer, in a lot of cases, and coding up an application. So they need a lot of these other components, they need access to servers with certain software running on it already such as web servers, messaging servers, database servers, things like that, and they need the development software itself. So Platform as a Service provides all of this so that they don't have to set up database servers, messaging servers, web servers, and so forth.

***Software as a Service*** is subscribing to the finished software itself. So that goes back to that customer management software that we just spoke about, wherein you simply do not want to have to bother purchasing, installing, updating, and maintaining the software that you need. You can simply subscribe to it because somebody else has that software. So you can simply access it usually over the Internet and simply gain access to the software in that fashion rather than installing it in your own organization. 

If one has to sum up whole idea behind cloud computing, in layman terms, it is taking advantage of the infrastructure that's already in place by other organizations accessing these features in the form of a subscription.

## Key technologies that enabled Cloud Computing
There are several key responsible for the continued drive of development of cloud computing. 

* Cloud datacernter
* Virtualization
* Cloud APIs
* Cloud storage
* Cloud database

Cloud datacenter provides the scalability and the reliability required for cloud computing. The providers particularly the larger scale ones are organizations such as Microsoft, and Amazon, and IBM, very large organizations with has "unlimited" resources when it comes to being able to supply for a single customer. You know, you can really have as much as you need. They are strategically located as well in cost-effective areas so that you can find services that are physically close to you. 

Virtualization is possibly the most important technological driver in cloud computing and it is simply the ability to have multiple, virtual operating systems running on a single physical computer. The hardware these days is capable of so much more than what it was say 15 or 20 years ago. So back then, if I wanted 5 servers, I would have to purchase 5 physical computers. These days I need only purchase one and I can install multiple operating systems on that single physical computer. That's the idea of virtual machines. So if I want more servers from a cloud type of environment, they just fire up additional virtual machines. Cloud-based services and virtualization combined it's generally offered under a pay-as-you-go pricing model. So you only pay for what you use. You don't have to buy a package, for example, of ten virtual machines if you only need two. So it helps to keep your cost down even further. 

Cloud API's or Application Programming Interface serves as this layer between users, and the cloud services, and resources that they require. The most commonly used API architecture these days is what's known as Representational State Transfer, or REST for short. For example, if they have to connect to a database to obtain information for their application, the REST architecture is how they can do that. It's a much more light weight and consistent type of architectural environment and fully supported under cloud services. 

Cloud storage is typically accessed using any kind of web-based HTTP interface that is offered by the cloud service providers. An application that has to gain access to data, well, the data is up in the cloud and their application as long as it has Internet connectivity, they can use standard HTTP request to access that data. And with it being in the cloud and accessed over a web-based interface, it doesn't matter where they are or where the application is. As long as it has access to the Internet, it has access to the data. 

Cloud database mostly refers to the NoSQL databases that the cloud providers offer as a service, which has such a high scalability.



## Attempting to define Cloud Computing

Cloud computing in and of itself is a little bit difficult to define.The way that any cloud service provider looks today might be significantly different a couple of years from now simply because it does change so rapidly. With that in mind there is a loose definition put forth by the National Institute of Standards and Technology.

> A model for enabling convenient, on-demand network access to a shared pool of configurable computing resources such as networks, servers, storage, applications, and services, that can be rapidly provisioned and released with minimal management effort or service provide interaction. 

That is a fairly convoluted definition. However what we mostly have to get out of it  is simply the fact that any given organization has whatever infrastructure in place to meet their needs. If that infrastructure is robust enough, then not only can they meet their needs, they can also meet other people's needs. And a simple example of this is if you think about web sites, let's say that you have an organization and you have a web server, you can obviously host your web site. But if it's a fairly simple web site and it's not really heavy to run, well, obviously you could host another web site on that same server. Say you have a friend who wants to also have a web site. Well, you can say I have a web server, let's put your web site on my server. And if it's another business you might charge them a bit of a fee for that. That in essence is a cloud service because the site itself resides on a server that is physically separate from site owner.

They access it over the Internet. They don't really care where the physical server is and they simply pay a subscription fee each month to be able to continue to have that web site up on that server. That is, in essence, cloud computing. All of the resources reside with someone else. They simply pay for access to those resources. From that point, all they need to be concerned with is the management of the web site itself. They will rely on you to keep the server secure and up to date, patch the operating system, upgrade the applications, things like that. Make sure that the security software is maintained. So you know, again, you alleviate those concerns from the customer. You just say, here, here's some space on my web server. Go ahead and post your site, we'll worry about everything else. That's the basis behind cloud computing.

With that in mind to define something, we must be able to describe it. Or in other words say it's core attributes. When talking about cloud computing service, we see generally note the following attributes:

* elasticity,
* on-demand,
* provider pooled computing resources
* metered service usage
* broad accessibility



***Elasticity*** refers to the ability to very rapidly add (or what is refered as provision), and/or remove (deprovision), resources and/or services as needed. A very common example of that is a virtual machine itself. So if I have my subscription through to my cloud service provider, I can simply create and configure a new virtual machine, literally within minutes. You can provision a new one any time you need it. Similarly, if you determine you no longer need it, you can deprovision it and that is entirely up to you. It is very flexible, hence the term elasticity.

***On-demand*** cloud services are available 24 hours a day, 365 days per year from anywhere given network connectivity. So basically, as long as you have access to the Internet, you have full and complete access to your cloud computing environment. There are effectively no business hours when it comes to cloud computing. 

***Provider pooled computing resources***, a cloud provider simply acquires and maintains all of the computing hardware on which the cloud services run and you share that amongst all of your customers. If I'm the provider, it's up to me to acquire all of the hardware I need to be able to support all of my users. But from that point, it would be certainly far too expensive if I were to dedicate physical servers to each of my customers.Customers share the physical. Now most providers do give you the option to have dedicated resources as well, but of course, they would be more expensive. So the pooled typically helps to save on money.

***Metered service usage*** allows for the subscription fee to be based on a billing model that is per use. So in other words, you only pay for what you use. Typically things like the amount of storage space you use and/or the amount of network bandwidth consumption will determine what your bill is each month. In other words you do not pay for what you do not use. 

***Broad accessibility***, this means that in essence, the cloud services can be accessed from any type of device. There really isn't any limitations there. If the device itself can access the Internet, it can in turn access whatever storage, whatever service, whatever application is available as part of your subscription. So desktop, laptops, mobile devices, any kind of tablet or smartphone, all of those are able to access the cloud services environment.

## Core Cloud Computing Service Models (Core 3 + 1 "new")

In [begining of this post](#history-of-cloud-computing) I hinted about the core 3 service modesl - IaaS, PaaS and SaaS. They are also known as the three primary cloud service models. So to sum them up:

* IaaS, which is Infrastructure as a Service. This is typically targeted at the IT managers not so much the end users. 
* Platform as a Service or PaaS, this is targeted more so at your IT developers and those who deploy software.
* SaaS for Software as a Service, which is targeted at the end users and not so much the IT admins or the developers. 

***IaaS***, Infrastructure as a Service, this is typically concerned with the IT compute resources on which the other cloud services run. Those resources are available remotely over the network. But a perfect example of IaaS is a virtual machine. You do have to have some kind of server to host storage or host an application. So the virtual machine itself provides that infrastructure. On a more basic level, of course, it's the physical server itself on which that virtual machine is running. But as a customer, you're not really concerned about that. We just want to be able to create a new virtual machine. The provider manages the hardware that ultimately drives that system. So virtual machines, storage, virtual networks all of those would fall under the category of IaaS. 

**PaaS**, Platform as a Service, typically targeted at the developers. Developers these days need a robust environment to develop their applications. They need to test them. They need to deploy them. They need to make sure that they are going to support the needs of the users. So you can certainly create your own development environment within your organization or you can simply subscribe to it in the cloud. They will provide the necessary virtual machines. They will provide all of the necessary services such as messaging or database, things like that. And they will even provide a means to test and deploy that application.

***SaaS***, Software as a Service, is specifically for the end users to be able to perform particular business tasks. The service is delivered remotely over a network, but the users can use any type of device. User device requires only a thin client, which means they only have to have connectivity to whatever is supplying the service. Now users may have limited application configuration settings, but in a lot of cases, that is exactly what you want. You don't want the user to have control over the configuration, that's done by administration. 

But all of them can be put together. You can have any or all of these services as part of your subscription. You don't generally go in and say I want an SaaS subscription. It really does comes down to what you use. And again, with all cloud services, you only pay for what you use. 

And with that in mind lets talk about a relatively new approach emerging now for cloud computing service models and that's what's known as ***XaaS***, where the X actually stands for Anything as a Service. So any IT service that's hosted on a provider infrastructure can then be offered as a cloud service model. A typical example of the services you will typically find under XaaS are: 

* Data as a Service
* Storage as a Service,
* Communication as a Service
* Business Processes as a Service

Data as a Service, the data is available on demand and encrypted in transit. So it might be the case where you have maybe a single application or even multiple applications, but they're all accessing a single copy of, maybe, a database. So the geographic location is irrelevant as long as there is Internet connectivity with the devices that are accessing it. They all access that central copy of that data, so they can use anything, it doesn't matter. But it's that same single version of a data file that everyone is accessing. So all that is required from the perspective of a customer is simply just data.They don't need virtual machines. They don't need additional storage. They don't need web sites. They just simply need access to data in a very centralized means, but accessible from anywhere. 

Storage as a Service, a lot of document management type of companies might offer this as a service, it is simply offsite data backup storage and disaster recovery. A lot of companies need even with respect to governmental regulations, they need to store their data offsite somewhere other than their own organization. So if they don't have that, you can subscribe to somebody else's services. 

Communication as a Service, is the communications infrastructure that we are all fairly familiar with, typically Telcos, telephone companies. So the infrastructure is hosted on the provider equipment. We can still remotely access that infrastructure over our network. But the complex equipment setup is handled by the provider as is fault tolerance. It's up to them to make sure they are providing us that higher level of fault tolerance. And it's pay-as-you-go, like any kind of other subscription-based service, so Voice over IP is a very good example of this. You know, most businesses don't have the infrastructure to implement a VoIP system. So you simply subscribe to those who do have the necessary infrastructure.

Business Processes as a Service, this is outsourced business process automation. This typically relies on the existing cloud infrastructure services such as SaaS, PaaS, and IaaS. But the business processes are delivered over the cloud, and the complexity of those process and their automation runs elsewhere. So this allows for a faster time to market and any kind of regulatory compliance is still handled by the provider.

## Summary

Cloud computing is powerful and expansive and will continue to grow in the future as well as provide many benefits.

Will it be "the future"? Who knows, probably not, as it outgrows itself with the pace that this article would be outdated in a few months.   Regardless it is always nice to think about, as well as reflect, on the past, and how have things changed. Check this video from 2010 about the [cloud](https://youtu.be/M7H1NrR4Lb4?t=90).



