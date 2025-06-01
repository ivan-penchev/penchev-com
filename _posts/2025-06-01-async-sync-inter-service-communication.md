---
title: "Synchronous or Asynchronous? How to Choose the Right Backend Architecture"
author: ivan
date: 2025-06-01 12:22:21 +0100
categories: [Software Architectures]
tags: [Software Architectures, Cloud Native]
toc: true
mermaid: true
math: true
---

One of the most critical decisions in backend engineering is choosing between synchronous and asynchronous inter-service communication. 
The right choice isn't about dogma; it's about understanding trade-offs. Getting it wrong can lead to a "distributed monolith" - a system with the complexity of microservices but the tight coupling of a monolith. 
Let's break down when to use each approach.

## The Case for Simplicity: Start with Synchronous

By default, synchronous architecture is the way to go. It’s a simple, request-response model where the client sends a request and waits for the server to process it and send back a response. It's often the natural starting point for new projects or less experienced teams.

Think of synchronous communication like a phone call: one service or component sends a request and then waits patiently for the response. If the other side doesn’t pick up, the call fails.

* Why it's great:
	* Easy to Understand: The flow is linear and easy to trace. `Service A` calls `Service B` and waits for a response.
	* Simple Debugging: Errors are returned directly in the response, making it easy to pinpoint failures.
	* Lower Initial Complexity: No need for extra infrastructure like message brokers.
* When it falls short:
	* When a slow downstream service can cause cascading failures, blocking your entire system.
	* When you need to perform heavy, long-running tasks without making the user wait.

## The Power of Resilience: When to Go Asynchronous

Asynchronous flows resemble texting or publishing on social media, you put something up, and at some point of time, someone would reply or send a like.
Asynchronous architecture decouples services. Instead of waiting for a direct response, services publish messages or events to a message broker. Other services can then process these messages in the background.

* Why it's great:
	* Improving Scalability and Performance: Absorb traffic spikes by queuing requests. Your system can process them at its own pace, preventing overload. For example, during a flash sale, you can queue up all incoming orders and process them as fast as you can, rather than dropping requests.
	* Increasing Resilience: If a service goes down, messages remain safely in the queue. Once the service is back online, it can resume processing, ensuring no data is lost. This is what the podcast hosts call a "self-healing" system.
	* Integrating with Unreliable Third-Parties: Don't let a flaky external API for sending marketing emails break your critical user registration flow. Publish a `UserRegistered` event and let a separate, non-critical service handle the API call.
* When it falls short:
	* Eventual Consistency - Data may not be up-to-date instantly across all services.
	* Debugging Complexity - "Where did that message go?" logs, traces, and queue metrics become essential.
	* Tougher Error Reporting to End Users - If something fails in the background, the user might not get an immediate status update.
	* The "Distributed Monolith" - A common anti-pattern where services are broken apart but remain tightly coupled through synchronous, blocking calls, inheriting the downsides of both monoliths and microservice

## Essential Async Patterns and Tools

When you go async, you'll encounter these concepts:

* Message Brokers: The backbone of async systems.
	* RabbitMQ: A versatile and mature choice for complex routing.
	* Apache Kafka: A powerhouse for high-volume, real-time data streaming.
	* NATS: A simple, high-performance open-source messaging system for cloud-native applications, IoT, and microservices architectures.
	* Google Cloud Pub/Sub: A fully-managed, scalable messaging service from Google Cloud that allows you to send and receive messages between independent applications.
* Events vs. Commands: Naming matters!
	* Command: Tells a service to do something. It's a request.
		* Example: A UI sends a `CreateUser` command containing the user's email and password to the `User Service`. It expects one service to act on it.
	* Event: Notifies the system that something happened. It's an immutable fact.
		* Example: After the user is saved, the `User Service` publishes a `UserCreated` event. The `Notification Service`, `Analytics Service`, and `CRM Service` can all independently listen for this event to do their jobs, without the `User Service` needing to know they even exist.
* Architectural Patterns for Success:
	* CQRS (Command Query Responsibility Segregation): Separate your "write" models (Commands) from your "read" models (Queries).
	* Event Sourcing: Store a log of all events that have occurred. This provides a full audit trail.
	* Hexagonal Architecture (Ports and Adapters): An architectural pattern that decouples the core application logic from external concerns like databases, UI, or message brokers, making it easier to switch between sync and async communication.
* Event Schemas:
	* Protobuf (Protocol Buffers): A binary serialization format developed by Google. It is language-neutral, platform-neutral, and highly efficient for serializing structured data
	* Apache Avro: A data serialization system that uses a JSON-based schema. A key feature is that the schema is sent along with the data, which can be useful for schema evolution.
	* CloudEvents: A specification for describing event data in a common way. Its goal is to simplify event declaration and delivery across services, platforms, and beyond.

## The Hybrid Approach: The Best of Both Worlds and The Ultimate Recommendation

The final takeaway is clear: don't force one pattern on everything. The most robust systems use a hybrid approach. Use synchronous calls for fast, simple, and critical operations. Introduce asynchronous communication strategically to solve specific problems related to scale, resilience, or long-running tasks. Your architecture should be a living thing that evolves with your product or system's needs.

Example: A Food Delivery App

* Synchronous calls:
	* Fetching Menus: When a user opens a restaurant page, a synchronous API call is made to get the menu. It's fast, and the UI needs the data immediately to render the page.
	* User Authentication: Logging in is a synchronous process that must complete before the user can proceed.
* Asynchronous flow:
	* Placing an Order: When the user clicks "Place Order," the API can synchronously validate the order and authorize the payment. Once successful, it returns an immediate 202 Accepted and publishes an OrderPlaced event.
	* From there, multiple services react asynchronously:
		* Restaurant Service listens to notify the restaurant of the new order.
		* Notification Service listens to send the user an "Order confirmed" email/push notification.
		* Driver Dispatch Service listens to start finding a nearby driver.
	* The user's app can then poll a GET /api/orders/{id} endpoint or use WebSockets to get real-time status updates as the async processes complete in the background.