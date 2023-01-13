---
author: n19
datetime: 2023-01-13
title: Getting started with ngrx
slug: getting-started-ngrx
featured: true
draft: false
tags:
  - ngrx
ogImage: ""
description:
  ngrx. What it is and how to use.
---

## Table of contents

# Getting Started With NGRX in Angular

Getting started with ngrx in angular can be a fairly straightforward process. It is important to understand the basics of the library before diving in too deep. This article will cover some of the basics of ngrx, including what is and why it is useful, discussing some of the data flow concepts involved, and also providing an example of how to get started with it.

## What is NGRX?

NGRX is a library that helps manage state in angular applications. It is based on the concept of a redux store, which is a centralized data store where all of an application's state is stored in one location, rather than scattered throughout the application and possibly stored in different services or components. NGRX helps to make sure that state is maintained across multiple components and services, which helps to keep the application more manageable.

## Why Use NGRX?

NGRX helps applications remain scalable and maintainable by keeping state centralized and allowing components to communicate with each other without directly altering each other's state. This communication is achieved through ngrx's "action-reducer" pattern, which allows components to emit "actions" and have a "reducer" to handle the action and update the state accordingly.

This pattern helps to ensure that components are not directly manipulating each other's state, which allows for better debugging and maintainability. It also allows for better separation of concerns and a single source of truth when working with data.

## Data Flow Concepts

The action-reducer pattern is the cornerstone of ngrx. It is based on the idea that each component or service can emit an action, which is captured by a reducer. This reducer can then be responsible for updating the state of the application. This data flow works by allowing components to emit actions, which are then captured by the reducer and used to update the state of the application.

## Getting Started with NGRX

Before diving in to using ngrx, it is important to make sure that you understand the basics of redux, which ngrx is based on. 

Once a basic understanding of redux has been achieved, the next step is getting ngrx set up in your angular application. This can be done using the ngrx/store package. This package provides many of the necessary functions to manage an application

# Redux

Redux is a library used in JavaScript apps that helps make managing data simple and consistent. It does this by providing an efficient way to store and access data, as well as dispatch actions to change data. 

## Key Concepts

The core concepts of Redux are actions, reducers, and store.

### Actions

Actions are plain objects that are used to send data from your app to your store. They typically include a `type` field, which is a string that describes the action being performed. Actions are dispatched by a function called an action creator.

### Reducers

Reducers are responsible for taking in an action and the current state of the store, and returning a new state. Reducers must be pure functions, meaning that they should always return the same output for a given set of inputs.

### Store

The store houses all the data pertaining to your application, and is the only source of truth. The store is composed of the root reducer, state, and any middleware. Middleware give you an opportunity to intercept dispatched actions and do something with them before they reach the reducers. 

## Benefits of Using Redux

- **Predicable State Management:** Redux ensures that the state of your application is predictable, allowing for easier and faster debugging. 
- **Efficient Data Access:** Redux allows for easier and faster access to data, since everything is managed in the same place.
- **Separation of Concerns:** Redux allows for separation of concerns between UI and data logic, making for more efficient and resilient applications.
- **Cleaner Code:** Using Redux leads to cleaner code as it removes the need for complicated logic for data management. 

Using Redux in your application can help make it more efficient and easier to manage data.

# Setup ngrx in an angular application

Setting up ngrx in an Angular application can be done by following these steps:

Install the ngrx packages by running ```npm install @ngrx/store @ngrx/effects @ngrx/entity``` in the command line.

In the ```app.module.ts``` file, import the ```StoreModule``` and ```EffectsModule``` and add them to the ```imports``` array. Also, add the ```reducers``` to the ```StoreModule```'s ```forRoot``` method.
```ts
import { StoreModule } from '@ngrx/store';
import { EffectsModule } from '@ngrx/effects';
import { reducers } from './store/reducers';

@NgModule({
  imports: [
    StoreModule.forRoot(reducers),
    EffectsModule.forRoot([])
  ],
  ...
```
Create a new folder called ```store``` in the ```src``` folder. Inside the ```store``` folder, create a new file called ```app.state.ts``` and define the initial state of the application.

```ts
export interface AppState {
  // define the properties of your state here
}
```
Create new files inside the ```store``` folder for actions, effects, and reducers, and define the actions, effects and reducers accordingly.
```typescript
// actions.ts
import { createAction, props } from '@ngrx/store';

export const loadData = createAction(
  '[App] Load Data',
  props<{ data: any }>()
);

// effects.ts
import { Injectable } from '@angular/core';
import { Actions, createEffect, ofType } from '@ngrx/effects';
import { loadData } from './actions';
import { map, mergeMap } from 'rxjs/operators';

@Injectable()
export class AppEffects {
  loadData$ = createEffect(() =>
    this.actions$.pipe(
      ofType(loadData),
      mergeMap(action => 
        // make an HTTP call to load data
        this.http.get('/api/data').pipe(
          map(data => ({ type: '[App] Data Loaded', payload: data }))
        )
      )
    )
  );

  constructor(private actions$: Actions, private http: HttpClient) {}
}

// reducers.ts
import { createReducer, on } from '@ngrx/store';
import { loadData } from './actions';

export const initialState = {
  data: []
};

export const appReducer = createReducer(initialState,
  on(loadData, (state, { data }) => {
    return { ...state, data };
  })
);

```
In your component, import the ```Store``` and ```select``` from ```@ngrx/store``` and use it to subscribe to the state and dispatch actions.

```typescript
import { Component } from '@angular/core';
import { Store } from '@ngrx/store';
import { loadData } from './store/actions';
import { selectData } from './store/selectors';

@Component({
  selector: 'app-root',
  template: `
    <div *ng
    <div *ngFor="let item of data | async">
      {{ item }}
    </div>
    <button (click)="loadData()">Load Data</button>
  `
})
export class AppComponent {
  data = this.store.pipe(select(selectData));

  constructor(private store: Store) {}

  loadData() {
    this.store.dispatch(loadData());
  }
}
```
Finally, add the effects to the ```providers``` array in the ```app.module.ts``` file
```python
import { AppEffects } from './store/effects';

@NgModule({
  ...
  providers: [AppEffects],
  ...
})
export class AppModule { }
```

That's it! You have successfully set up ngrx in your Angular application. You can now use the store to manage the state of your application, handle actions, and perform side-effects using effects.
