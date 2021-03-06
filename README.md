<img width="200" src="https://raw.githubusercontent.com/luisvinicius167/dutier/master/img/logo.png"/> 

Dutier is a small (1kb), async and simple state management solution for Javascript applications. <br/>

[![npm package](https://img.shields.io/badge/npm-0.4.1-blue.svg)](https://www.npmjs.com/package/dutier)
[![CDN](https://img.shields.io/badge/cdn-0.4.1-ff69b4.svg)](https://unpkg.com/dutier@0.4.0)


### Influences
It evolves on the ideas of [Redux](https://github.com/reactjs/redux).

## Getting Started

### Install
* NPM: ``` npm install dutier ```
* CDN: ```https://unpkg.com/dutier@0.4.1```

### Features
 * small 1kb minified
 * simple, small learning curve
 * no dependencies
 * async reducers by default
 * promise based
 * inspired by Redux
 * tiny API.
 
 ### React Examples:
[![React with Dutier](https://codesandbox.io/static/img/play-codesandbox.svg)](https://codesandbox.io/s/1AyKXMpG)

With `Dutier` your `actions` are just pure functions that returns a payload information about how to work with the state, and the `dispatch` method always return new values based on your state.

Dutier `Reducers` are async by default.

```javascript
import { createStore, getState, dispatch } from 'dutier'

// sets your initial application state
createStore({ count: 1 }, reducer)

// your action
function increment( value ) {
  return { type: 'INCREMENT', value }
}
/**
 * Async Reducer
 * Each reducer receives the async dispatch method as first argument,
 * the current state and the action payload
 * To change your state, always use the async dispatch method
 */
function reducer( dispatch, state, { type, value } ) {
  switch (type) {
    case 'INCREMENT':
    // async or sync operation
      setTimeout( () => {
        dispatch(Object.assign({}, state, { count: state.count + value }))
      }, 2000)
    default:
      return state  
  }
}

 /**
 * Async actions
 * Use dispatch to return new values based on the state
 * tree. dispatch is async and returns a Promise with your action type and
 * the new state value.
 * After 2 seconds, will be displayed on console:
 * state -> {count: 2}, getState() -> {count: 2} 
 */
dispatch( increment(1) )
  .then( ({ type, state }) => console.log( state, getState() )) 
```


### The Gist
The application state is stored in an object tree inside a single store. Your actions will only dispatch information about how work with the state and then return new state values based on your state.

That's it!

```javascript
import { createStore, subscribe, dispatch, getState } from 'dutier'

/**
 * Set the initial store state in a single object.
 */
createStore( { count: 1 } )

/**
 * Actions are pure functions that return a payload
 */
function increment( value ) {
  return { type: 'INCREMENT', value }
}

/**
 * Async Reducer
 * Each reducer receives the async dispatch method as fisrt argument,
 * the current state and the action payload
 * To change your state, always use the async dispatch method
 */
function reducer( dispatch, state, { type, value } ) {
  switch (type) {
    case 'INCREMENT':
    // async or sync operation
      setTimeout( () => {
        dispatch(Object.assign({}, state, { count: state.count + value }))
      }, 2000)
    default:
      return state  
  }
}
    
/**
 * You can use subscribe() to update your UI in response to actions;
 * The subscribe are just be called if the state was changed.
 */
 componentWillMount() {
  this.unsubscribe = subscribe( { type, state } ) => {
    console.log('Reducer new state value ', state, 'Store state: ', getState())
  })
 }


/**
 * Use dispatch to return new values based on the state
 * tree. dispatch is async and returns a Promise with your action type and
 * the new state value.
 */
dispatch(increment( 1 ))
 .then( ({ type, state }) => {
   console.log(`The value is: ${getState().count}`) // 2
 })
```

### Simple and efficient API.

Dispatch
 * Trigger an action to do something with the state. It's async by default and always return a promise <br> 
 that contains the action type and the new state value 

```javascript
/**
 * @name dispatch
 * @description Trigger some action.
 * @param { Function } The function that return your action payload
 * @return {Promise} Return a Promise with the action payload
 */

// On your component
import {dispatch} from 'dutier'

// You can receive the response of your action and do something, or not.
// If you want, you can chain the dispatch Promises.
dispatch( increment(1) )
  .then( ({ type, state }) => {
    console.log(type, state)
  })
```

Actions
 * Actions are payloads of information that send data from your application to your store. They are the only source of information for the store. You send them to the store using dispatch().
 
```javascript
function increment( value ) {
  return { type: 'INCREMENT', value }
}
```


Store State
 * Set the initial Application store state.
```javascript
/**
 * @name createStore
 * @description Set your initial Application store state
 * @param { Object } state Your application state data
 * @param { Function } reducers Your store reducers
 */
 
import { createStore } from 'dutier'

createStore( { count: 1 }  [, ...reducers] )
```

Getting the store state
 * Get the current state value
```javascript
/**
 * @name getState
 * @description Get the current state value
 */
 
import {getState} from 'dutier'

getState() // returns a copy of your store state { count: 1 }
```

Combine
 * Combine your store reducers
```javascript
/**
 * @name combine
 * @param {Function} Register your reducers
 */
 
import { combine } from 'dutier'

function reducer( dispatch, state, { type, value } ) {
  switch (type) {
    case 'INCREMENT':
      dispatch({ count: state.count + value })
    default:
      return state  
  }
}

function otherReducer( dispatch, state, { type, value } ) {
  switch (type) {
    case 'ADD':
      dispatch( { count: value } )
    default:
      return state  
  }
}

combine( reducer, otherReducer, [, ...reducers ])
```


Subscribe
 * It will be called any time an action is dispatched and just if the state was changed.
```javascript
/**
 * @name subscribe
 * @param { handler } handler A callback function that will be triggered when
 * your state change
 */
 
import {subscribe, getState} from 'dutier'
  
  componentWillMount(){
     // Subscribe to changes on your store, do something with the value.
     this.unsubscribe = subscribe(( { type, state } ) => {
       this.setState( { count: getState().count } )
     })
  }
  
  componentWillUnmount() {
    this.unsubscribe()
  }
```

That's all folks!


#### License
MIT License.
