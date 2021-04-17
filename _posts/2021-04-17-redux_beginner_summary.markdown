---
layout: post
title:      "Redux Beginner Summary"
date:       2021-04-17 22:08:56 +0000
permalink:  redux_beginner_summary
---


This blog will cover the basic core concepts of Redux and the patterns to follow when using Redux

## What is Redux?

**Redux** is a state management JS library used for managing the global state of an application. The **state** describes the condition of an app at a specific point in time. Managing the state of an app is one of the hardest things to control during front end development. To facilitate this task, Redux provides a centralized store that can contain the global state of an application, allowing any component of the application to subscribe to the store to get or update data. It also provides a set of rules or patterns to follow to ensure that the global state of the app is being updated in a predictable fashion. 

>" The patterns and tools provided by Redux make it easier to understand when, where, why, and how the state in your application is being updated, and how your application logic will behave when those changes occur. " (from Redux doc)

## Pieces of Redux

### Actions

An **action** describes an event that happens on the user interface like the load of a page or click of a button or link.
It is a JavaScript object that contains a `type` property, which is a description of how the state should be updated. The action object may also have a `payload` field that contains the info needed to update the state.

Example of an action object:

```js
const updateHoursAction = {
  type: 'time/hoursUpdated',
  payload: new Date().getHours()
}
```

### Action Creators

An **action creator** is a typical JS function, that may or may not take an argument (usually the piece of data needed to update the state), and return an **action** object. The action creator is typically used to alleviate the task of writing out an action object by hand everytime it is needed. Bellow is an example of an action creator function that returns the `updateHoursAction` object created above:

```js
const updateHours = newHours => {
  return {
    type: 'time/hoursUpdated',
    payload: newHours
  }
}
```

### Reducers

A **reducer** is a function that handle the logic of how to update the global state of the application, given the current state and the type of action that happened. Given the current state and an action object, the reducer function evaluates how to update the state and returns the **new state** if it cares about the action that occured on the UI, or the **unchanged existing state** if it does not care about the action that happened.

>Reducers must always follow some specific rules:
*  They should only calculate the new state value based on the state and action arguments
* They are not allowed to modify the existing state. Instead, they must make immutable updates, by copying the existing state and making changes to the copied values.
* They must not do any asynchronous logic, calculate random values, or cause other "side effects"

Here is an example of a reducer function defining the logic for updating a time state:

```js
// initial time state
        const initialTimeState = {
            hours: new Date().getHours(),
            mins: new Date().getMinutes(),
            secs: new Date().getSeconds()
        };
        // time reducer
        function timeReducer(state = initialTimeState, action) {
				// Check to see if the reducer cares about this action
				// If so, make a copy of `state` and update the copy with the new value when the conditions are met
            switch (action.type) {
                case "time/hoursUpdated":
                    if (state.hours !== action.payload) return {...state,
                        hours: action.payload
                    };
                case "time/minsUpdated":
                    if (state.mins !== action.payload) return {...state,
                        mins: action.payload
                    };
                case "time/secsUpdated":
                    if (state.secs !== action.payload) return {...state,
                        secs: action.payload
                    };
                default:
                    return state;
            }
        }

```

### Redux Store

The **store** is the place housing the global state of your application. An instance of a store can be created by calling the Redux library `createStore` API and passing it the reducer function. The `createStore` method will then use the reducer function to generate the initial state, and to calculate any future updates.

```js
const store = Redux.createStore(timeReducer);
```

The store is a JS object with special functionalities: `getState` and `dispatch` methods.

#### getState Method

The `getState` method is used to retrieve the current state from the store

```js
const state = store.getState();
```

#### dispatch Method

The dispatch method acts as a notifier. It is used to notify the store of an action that has happened, so that the store can run its reducer function to reevaluate and save the new state.

> "The only way to update the state is to call store.dispatch() and pass in an action object." 

```js
// We use the updateHours action creator function defined above to dispatch the `"time/hoursUpdated"` action to the store:

store.dispatch(updateHours(new Date().getHours()));

// We can retrieve the update hours by calling `getState`
console.log(store.getState().hours); // will print out the current hour value
```

## Redux Data Flow
Redux follow a **one way data flow** described as follow:

* Initialization:
 -- An instance of a store is created using a defined `root reducer`, which is called once to initialize the state of the application
 -- The UI accesses the current state of the Redux store and renders based on that state. Plus, the UI also subscribes to the store for any update so that it will be notified when the state changes and rerender accordingly.
 
* Update:
 -- When an event happens on the app, such as a page load or a click of a button
 -- The app `dispatch`es the action that happens to the `Redux store`
 -- The `store`  then runs the `reducer` function again with the `previous state` and the dispatched `action`, and saves the return value as the new `state`
 -- Next, the store notifies the subscribed UI of the update that happened so that it can rerender accordingly (depending on whether the data needed has changed or not)
 
 
 ## Redux App Example
 
 This example (time diplayer) only uses `HTML`, `vanilla JS` and `Redux API` to update the time on a page
 
 ```html
 <!DOCTYPE html>
<html>

<head>
    <meta charset="utf-8">
		<title>Redux basic example</title>
		<!-- Redux API -->
    <script src="https://unpkg.com/redux@latest/dist/redux.min.js"></script>
    <style>
        div {
            display: flex;
            flex-wrap: wrap;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            margin-top: 10%;
            margin-left: 25%;
            border: 3px solid brown;
            width: 50%;
        }
    </style>
</head>

<body>
    <div>
        <p>
            <span style="color:blue">Time: </span>
            <button style="background-color: peachpuff;" id="hours">00</button> :
            <button style="background-color: peachpuff;" id="mins">00</button> :
            <button style="background-color: peachpuff;" id="secs">00</button>
        </p>
    </div>

    <script>
        // Define the initial time state for the current time
        const initialTimeState = {
            hours: new Date().getHours(),
            mins: new Date().getMinutes(),
            secs: new Date().getSeconds()
        };
				
				// Create a " time reducer" function that determines what the new state
        // should be when something happens in the app
   
        function timeReducer(state = initialTimeState, action) {
				    // Reducers usually look at the type of action that happened
            // to decide how to update the state
            switch (action.type) {
                case "time/hoursUpdated":
                    if (state.hours !== action.payload) return {...state,
                        hours: action.payload
                    };
                case "time/minsUpdated":
                    if (state.mins !== action.payload) return {...state,
                        mins: action.payload
                    };
                case "time/secsUpdated":
                    if (state.secs !== action.payload) return {...state,
                        secs: action.payload
                    };
                default:
								    // If the reducer doesn't care about this action type,
                    // return the existing state unchanged
                    return state;
            }
        }

        // Create a new Redux store with the `createStore` method,
        // and use the `timeReducer` to update the logic
				
        const store = Redux.createStore(timeReducer);

        // get the element from UI containg the value for hours, mins, and secs
        
        const hoursElement = document.getElementById("hours");
        const minsElement = document.getElementById("mins");
        const secsElement = document.getElementById("secs");

        // update the UI whenever the state contained in the store changes
        function render() {
            // retrieve the current state from the store with `getState`
						const state = store.getState();
            // destructure the hours, mins and secs value
            const {
                hours,
                mins,
                secs
            } = state;
						// format of the time to be diplayed
            hoursElement.textContent = hours.toString().length === 1 ? "0" + hours.toString() : hours.toString();
            minsElement.textContent = mins.toString().length === 1 ? "0" + mins.toString() : mins.toString();
            secsElement.textContent = secs.toString().length === 1 ? "0" + secs.toString() : secs.toString();
        };
        // update the UI with the current state
        render();
        // Subscribe the UI to the store so that it rerenders only when the state changes
        store.subscribe(render);
				
        //  Callback function to "dispatch" action objects,
        // which should describe "what happened" in the app
       
        function updateTime() {
            store.dispatch({
                type: "time/hoursUpdated",
                payload: new Date().getHours()
            });
            store.dispatch({
                type: "time/minsUpdated",
                payload: new Date().getMinutes()
            });
            store.dispatch({
                type: "time/secsUpdated",
                payload: new Date().getSeconds()
            })
        }
				
				// asynchronuously dispatch actions every second to update the state to reflect the current time
        setInterval(updateTime, 1000);
    </script>
</body>

</html>
 ```
 
 Now, run this html file on your browser and you will see the time constantly updating on the page.
 
 You can learn more about Redux fundamentals  on [Redux docs](https://redux.js.org/tutorials/fundamentals/part-1-overview).
 
 Thank you for stopping by today
 
 Until next time,
 
 Happy coding/learning !!
