---
author: n19
datetime: 2023-01-10
title: Getting started with ngrx/store
slug: getting-started-ngrx-store
featured: true
draft: false
tags:
  - ngrx, ngrx/store
ogImage: ""
description:
  ngrx/store. What it is and how to use.
---

## Table of contents

# Introduction
NGRX Store is a library for managing state in Angular applications. It is built on top of RxJS, which is a library for reactive programming using Observables. NGRX Store provides a way to organize your application's state into a single, immutable data store, and allows you to manage and update the state using a set of powerful tools and concepts.

NGRX Store uses a centralized store to hold the state of your application, which makes it easy to share the state between different components and services. The state is represented as an Observable, which makes it easy to subscribe to changes and react to them.

NGRX Store also provides a set of powerful tools for managing the state, such as Actions, Reducers, and Effects. Actions are used to represent events that occur in the application, such as a user clicking a button. Reducers are used to update the state based on the actions that occur. And Effects are used to handle side effects, such as making an HTTP request.

By using NGRX Store, you can write more predictable, testable, and maintainable code. It also gives you a consistent way to handle state management across your application, which makes it easier to reason about the behavior of your application.

NGRX also have other packages such as NGRX Effects, NGRX Entity, NGRX Router-Store, etc. which work as extensions to the base NGRX/store and provide more specific functionality.

# Benefits of using ngrx/store
There are several benefits to using NGRX Store in your Angular applications:

* Centralized state management: By using a centralized store to hold the state of your application, it becomes much easier to share the state between different components and services, and to reason about the behavior of your application.

* Immutable state: NGRX Store uses a strict set of rules for managing the state, which ensures that the state is always immutable. This makes it easy to track changes to the state and to undo or revert to previous states if necessary.

* Predictable behavior: NGRX Store uses a strict set of rules for managing the state, which makes it easy to predict how the state will change in response to different actions. This helps to make your application more testable and maintainable.

* Powerful tools: NGRX Store provides a set of powerful tools for managing the state, such as Actions, Reducers, and Effects. These tools make it easy to handle different types of state changes, and to handle side effects such as making an HTTP request.

* Reactive programming: NGRX Store is built on top of RxJS, which provides a powerful set of tools for reactive programming. This makes it easy to subscribe to changes in the state and to react to them in a consistent way.

* Scalable: NGRX's architecture is scalable for large and complex application with multiple modules and features. It also handle well in application with high data loads, with good performance.

* Testable: NGRX's architecture is testable, due to the separation of state and logic, making it easy to test the different part of your application, such as actions, reducers and effects.

* Well established: NGRX is one of the well-established and most popular state management library for Angular application, which is backed by a large and supportive community, with plenty of tutorials and resources available.

# Installation
Installing NGRX Store in an Angular application is a two-step process:

* Install the NGRX packages: You will need to install the @ngrx/store package, as well as any other packages you want to use, such as @ngrx/effects or @ngrx/entity. You can install them using npm or yarn.

```
npm install @ngrx/store @ngrx/effects @ngrx/entity
```

```
yarn add @ngrx/store @ngrx/effects @ngrx/entity
```

* Import the StoreModule in your app module: After installing the NGRX packages, you will need to import the `StoreModule` in your app's root module and configure it to use the `reducer` function that you will create.

```ts
import { StoreModule } from '@ngrx/store';
import { reducer } from './reducers';

@NgModule({
  imports: [
    StoreModule.forRoot({ state: reducer })
  ]
})
export class AppModule {}
```
Note that in the above example I'm importing reducer function from a `reducers.ts` file. This is where you will define all the logic of your state management using `Actions`, `Reducers`, `Effects`.

You also need to import the store in your component where you want to use it.

```ts
import { Store } from '@ngrx/store';

constructor(private store: Store) {}
```

It is also a good practice to organize the state and actions in a specific way, like creating a folder for each feature and a file for each action, reducer and effect.

You may also consider adding the ngrx dev tools to your browser to have a better developer experience

```
npm install @ngrx/store-devtools
```

```ts
import { StoreDevtoolsModule } from '@ngrx/store-devtools';
```

and then import it on your app.module.ts

```ts
imports: [
    StoreModule.forRoot({ state: reducer }),
    StoreDevtoolsModule.instrument()
  ]
```

It is also good to read the official documentation and the guidelines to have a deeper understanding of how to structure your state management and have a better organization and performance.

# Example
Here is a simple example of how you might use NGRX Store in an Angular application. Let's say we have a simple application that allows the user to increment or decrement a counter. Here's how we might set up the store to manage the state of the counter:

* Define the state: The first step is to define the state that the store will manage. In this case, we only have a single piece of state: the counter. We will create a file `counter.state.ts` to define the state like this:

```ts
export interface CounterState {
  count: number;
}

export const initialState: CounterState = {
  count: 0
};

```
* Create the actions: Next, we will create a file `counter.actions.ts` to define the actions that the store can handle. In this case, we have two actions: one to increment the counter and one to decrement it.

```ts
import { createAction, props } from '@ngrx/store';

export const increment = createAction('[Counter] Increment');
export const decrement = createAction('[Counter] Decrement');
```

* Create the Reducers: Now we need to create a file `counter.reducer.ts` to handle the actions and update the state.

```ts
import { createReducer, on } from '@ngrx/store';
import { increment, decrement } from './counter.actions';
import { CounterState, initialState } from './counter.state';

export const counterReducer = createReducer(initialState,
  on(increment, state => ({ count: state.count + 1 })),
  on(decrement, state => ({ count: state.count - 1 })),
);
```

* Import the StoreModule in your app module: After installing the NGRX packages, you will need to import the StoreModule in your app's root module and configure it to use the counterReducer function that you created.

```ts
import { StoreModule } from '@ngrx/store';
import { counterReducer } from './counter.reducer';

@NgModule({
  imports: [
    StoreModule.forRoot({ count: counterReducer })
  ]
})
export class AppModule {}
```
* Use the store in your component: Now you can use the store in your component by injecting it and subscribing to the state.

```ts
import { Component } from '@angular/core';
import { Store } from '@ngrx/store';
import { increment, decrement } from './counter.actions';

@Component({
  selector: 'app-root',
  template: `
    <button (click)="decrement()">-</button>
    <span>{{ count }}</span>
    <button (click)="increment()">+</button>
  `
})
export class AppComponent {
  count: number;

  constructor(private store: Store) {
    store.select('count').subscribe(count => this.count = count);
  }

  increment() {
    this.store.dispatch(increment());
  }

  decrement() {
    this.store.dispatch(decrement());
  }
}
```

# Scenarios where ngrx/store can be useful
NGRX Store is particularly useful in Angular applications that have a complex state or that need to handle a large amount of data. Here are a few scenarios in which NGRX Store can be useful:

* Large or complex state: NGRX Store provides a way to organize your application's state into a single, immutable data store, which makes it easy to share the state between different components and services. This can be particularly useful in applications with large or complex state that is difficult to manage using a traditional approach.

* Data-heavy applications: NGRX Store provides a way to manage large amounts of data in an efficient and performant way, which can be particularly useful in applications that need to handle a lot of data, such as large lists or tables of data.

* Real-time applications: NGRX Store's use of RxJS makes it easy to handle real-time data and updates, which can be particularly useful in applications that need to handle data in real-time, such as chat or monitoring applications.

* Applications that need undo/redo functionality: NGRX Store's immutability of state makes it easy to revert the state of the application to previous state, which can be useful in applications where undo/redo functionality is needed.

* Multi-module application : NGRX Store's ability to manage state at a global level in a centralized way and it’s powerful tools are also ideal for large-scale, multi-module applications where different sections of an application could have different ownership, but they still need to share the same data and state.

* Applications that need to handle complex state transitions: In some scenarios, an application might need to handle complex state transitions that depend on the current state, or that depend on the order in which actions are performed. NGRX's architecture allows you to handle these transitions in a clean and organized way.

Overall, NGRX store is useful in applications that need a powerful and predictable way to manage state, and that benefit from its separation of concerns, immutability and powerful tools.

# Downsides
NGRX Store is a powerful tool for managing state in Angular applications, but it also has some downsides:

* Learning curve: NGRX Store uses a set of concepts and tools that may be unfamiliar to some developers, such as Actions, Reducers, and Effects. It also makes use of RxJS, which may be unfamiliar to developers who haven't worked with reactive programming before. This learning curve can be steep, and it may take some time for developers to fully understand how to use NGRX Store effectively.

* Boilerplate: In order to set up NGRX Store, you will need to write a fair amount of boilerplate code, such as actions, reducers, and effects. This can add extra development time and make your codebase more complex.

* Performance: While NGRX Store can be performant, it does have some overhead, particularly when working with large amounts of data. It may also have performance issues with high-frequency updates. This can be mitigated by proper configuration and implementation of the NGRX store

* Over-engineering: NGRX Store is a powerful tool, but it may not be necessary in all cases. It's important to consider whether it's the right tool for the job before incorporating it into your application, as the added complexity and boilerplate may be unnecessary if the state is not complex or you don't need the features provided by NGRX.

* Steep developer requirement: As the technology is complex, the development team must have a good knowledge of RxJs, and the NGRX Store architecture, as well as the Angular platform, in order to use it efficiently, and to maintain the application with good performance and scalability.

It's important to consider these downsides when deciding whether to use NGRX Store in your application, and to weigh the benefits against the potential costs. In many cases, NGRX Store can be a powerful tool for managing state in Angular applications, but it may not be the best choice for every situation.

# How can I learn more?
There are many resources available to help you learn more about NGRX Store, here are a few places to start:

* Official documentation: The official NGRX Store documentation is a great resource to start learning about NGRX Store. It provides detailed explanations of all the concepts and tools, as well as examples of how to use them.

* Online tutorials: There are many online tutorials and courses available that can help you learn NGRX Store. Some popular platforms include YouTube, Udemy, and Pluralsight. These tutorials can help you learn the basics of NGRX Store and provide you with a solid foundation to build upon.

* Books: There are also some books that are available, such as "ngrx: in Action" which can help you dive deeper into the concepts of NGRX and how to use it in a real-world scenario.

* Community and Blogs : You can also find a lot of information on blogs and discussion forums, from tutorials to best practices, from people who have experience with NGRX Store and are willing to share their knowledge with the community.

* Try it out: The best way to learn is to try it out. Building a small scale application with NGRX Store will give you a much better understanding of how it works and how to use it effectively.

* NGRX's community: The NGRX community is strong and active, you can join the community on platforms like slack, gitter or stack overflow and ask questions and get help with the issues you are facing.

In addition to these resources, it's also important to keep in mind that learning is an ongoing process, you will learn more as you work with NGRX store and encounter new challenges and edge cases.

# Alternatives
There are several other libraries and tools available for managing state in Angular applications, here are a few alternatives to NGRX Store:

* Angular's built-in services: Angular provides several built-in services for managing state, such as ChangeDetectorRef, EventEmitter, and Subject. These services can be used to share data between components and services, but they lack some of the advanced features provided by NGRX Store, such as immutability, undo/redo functionality and centralized state management.

* Akita: It is a lightweight state management library, similar to NGRX store that is built on top of RxJS and it is designed to be simple and easy to use, it also provides powerful tools for managing the state, such as Actions, Reducers, and Effects.

* MobX : It is another popular state management library, which is not specific to Angular, but it can be integrated with Angular. It provides a simple and intuitive way to manage the state and it has a simple and easy to understand API, that make it easy to start with, but it lack immutability and undo/redo functionality by default.

* Apollo Client: It is mainly a GraphQL client, but it has a built-in state management solution as well, it can be a good choice for managing the state of data retrieved from a GraphQL server, it has a lot of features, but it's mainly optimized for GraphQL.

* Vanilla Redux: The original Redux is a library for managing state in JavaScript applications, it's not specific to Angular, but you can still use it in an Angular application. It provides a simple and predictable way to manage the state, but it requires more boilerplate to use it in Angular.

Ultimately, the best choice will depend on the needs of your specific application and the preferences of your development team. It's important to evaluate the features, performance and complexity of each library, as well as their adaptability to your use case, before making a final decision.

# Conclusion
NGRX Store is a powerful tool for managing state in Angular applications, and it's a well-established and widely used library. It provides a centralized state management approach, using immutability and a set of powerful tools such as Actions, Reducers and Effects. These tools provide many benefits such as predictability, testability, scalability, and making the application more maintainable.

However, like any tool, NGRX Store isn't a silver bullet, and it may not be the best choice for every situation. There is a learning curve and some added complexity associated with using NGRX Store, and it may not be necessary for applications with simple state. It's important to weigh the benefits and downsides of NGRX Store before deciding to use it in your application, and to consider the requirements of your specific use case.

In conclusion, NGRX Store is a powerful library for managing state in Angular applications, and it's a great tool for large or complex state, real-time applications, undo/redo functionality and multi-module applications. But it's important to understand its complexity, performance and requirements before deciding to use it, and to consider the alternatives that might suit better the application needs.