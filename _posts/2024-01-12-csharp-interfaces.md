---
layout: post
title: "Interfaces"
date: 2024-01-12
categories: [Programming]
tags: [csharp, dotnet, interfaces]
reading_time: 4
---

C# devs are just obsessed with patterns. Suppose I were to recommend a fool-proof business idea for a fellow .NET engineer. In that case, they should pick one of these SOLID, DDD, or CQRS acronyms or maybe modular monolith/hexagonal architecture and create a course on it.

There's a neverending debate on how to structure projects in your solution, and how a domain event should look. However, there's one thing common to almost all .NET codebases: interfaces are almost everywhere. And, to be honest, I don't really know why.

I've seen interfaces which only implementations were classes so simple, that they could've been rewritten into a simple static class with pure functions **without any usage or readability degradation**. I've seen countless enterprise `IRepository` with exactly one implementation. Have you seen the C# fundamentals [MSDN page](https://learn.microsoft.com/en-us/dotnet/csharp/fundamentals/types/interfaces) on interfaces? It's called "Interfaces - define behavior for **multiple** types". Again, I don't know why this interface obsession is a modern .NET engineer's modus operandi, but I did some mental wrestling and came up with a shortlist of "excuses".

## Unit tests

The most prevalent argument for creating "header\*" interfaces is that without them it's not possible to mock the underlying implementation. Well, I guess `Moq` made us lazy (that's good!), but there are more ways to test a unit of code than the de-facto-standard `Mock<ISuperficialDependencyInterface>`.

I've seen mocking libraries' ease of use backfiring on devs - it's easier to create a class with waaaay too much dependencies. But then, what's the point of unit testing a class that connects with 10 different subsystems? You can extract the core business logic to a humble object. It should be more testable if it doesn't make any database or HTTP calls. Even bether if it's public interface is pure functions only - testing such a class is pure joy.

Sidenote: extracting core logic by stripping it down from external dependencies and factoring out to a static class containing pure static methods is by far my favourite way of tidying code (see: [Tidy First book review]({% post_url 2024-01-08-tidy-first-book-review %})).

Besides, you can always provide virtual (a keyword often forgotten nowadays) methods, making it mockable using Moq without an explicit interface... or use a test double. A rather old-fashioned and cumbersome approach, but sometimes it's worth to reimplement a unit of code for test purposes, just to validate our implementation manually.

## Extensibility

Another argument for creating an interface mimicing the public contract of its only implementation: extensibility. Well, I've been using modern IDEs alongside vim for a long time. Extracting a new interface and substituting the class-type usage to an interface (e.g. in constructors that depend on it) seems like a 30 seconds of pattern-matching the correct IDE keyboard shortcuts.

Many of these extensible enterprise codebases have most of their interfaces being implemented by exactly **one** class. This (IMHO) leads to breaking the Interface Segregation Principle. If there's 1:1 matching between `IRepository` and `Repository` public methods, then it's easier to squash another member into the interface than divide it and reorganize areas dependent on it (e.g. constructors, factories).

I'm thinking of this as a very ahead-of-time code tidying. I think I'm in the opposite, just-in-time interface creation team.

## Interface as a contract

Interface definition may serve as a great way of exposing and documenting the contract between the interacting units. Yes, but I find it a reduntant abstraction when both the interface and its implementation are publicly visible. What's the point then? You can easily browse through a class's public members without writing another 10 lines of code.

I can live with public interface and internal, local to the assembly implementation, but a public interface with exaclty one **public** implementation indicates that the author mechanically outlined the contract in the easiest way the language allows.

## Dependency Injection

Less common, but still occurring. I realized that some devs create interfaces just so they can use the `IServiceCollection` extension methods. In this case, reading the docs can save creation of countless interfaces: [public IServiceCollection AddSingleton (this IServiceCollection services)](https://learn.microsoft.com/en-us/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions.addsingleton?view=dotnet-plat-ext-8.0#microsoft-extensions-dependencyinjection-servicecollectionserviceextensions-addsingleton-1(microsoft-extensions-dependencyinjection-iservicecollection)). Nuff said.

## Conclusion

I'm not against interfaces per se - these are a crucial OOP concept I use every day. That being said, not every class needs its interface counterpart, especially when it's a superficial header interface. There are various motivations for defining interfaces. What I'm against is mindlessly writing them, because you're used to/because moq requires it/because you'll need another repository implementation. Please don't.

\* Such an interface defined at top of the file containing class definition reminds me of C++ header files. And I don't think I do like this resemblance.
