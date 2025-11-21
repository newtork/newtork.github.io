---
title:  "HowTo: From Spring's TaskDecorator to Supplier"
date:   2025-11-20 08:00:00 +0000
modified: 2025-11-20 08:00:00 +0000 
comments: true
permalink: /weblog/2025/11/20/runnable-supplier/
categories: blog
tags: blog howto java
---

Recently I came across a minor inconvenience while integrating an extension to a Spring Boot application.
I was given an `@Autowired TaskDecorator` in [Spring](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/core/task/TaskDecorator.html).
The goal was to execute a `Supplier<T> task` within the decorator through a dedicated `ExecutorService`.

<!--more-->

And here's the solution that bridges both interfaces:

```java
// provided
@Autowired
TaskDecorator decorator;

// extension
Supplier<T> task;
ExecutorService executor;

// integration
AtomicReference<T> resultRef = new AtomicReference<>();
Runnable taskDecorated = decorator.decorate(() -> resultRef.set(task.get()));
Future<T> asyncResult = executor.submit(() -> {
  taskDecorated.run();
  return resultRef.get();
});
T result = asyncResult.get();
```

The `TaskDecorator` API only accepts Runnable.
And since we want to return a value, we'll need to capture the result in an `AtomicReference` object.