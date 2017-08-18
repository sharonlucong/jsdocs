 ### Actions
 - Actions (describe sth happened)

 `Actions` are payloads of information that send data from application to store. It must have a `type` property that indicates the type of action. You send them to the store using `store.dispatch()`

```ts
{
  type: "...",
  text: "..." // the structure of action object is up to the user
}
```

- Action Creators

functions that create actions

```ts
function addTodo(text) {
  return {
    type: ADD_TODO,
    text
  }
}
```

To Initiate a dispatch, we pass the result to the `dispatch()` function

```ts
dispatch(addTodo(text));
```

### Reducers
(specify how the application's state changes in response)

In Redux, all the application state is stored as a single object.

**You'll often find that you need to store some data, as well as some UI state, in the state tree. This is fine, but try to keep the data separate from the UI state.**

```ts
{
  visibilityFilter: 'SHOW_ALL',
  todos: [
    {
      text: 'Consider using Redux',
      completed: true,
    },
    {
      text: 'Keep all state in a single tree',
      completed: false
    }
  ]
}
```

- Handling Actions

The reducer is a pure function that takes the previous state and an action, and returns the next state.

```ts
(previousState, action) => newState
```

It's very important that the reducer stays pure. Things you should **never** do inside a reducer:

- Mutate its arguments;
- Perform side effects like API calls and routing transitions;
- Call non-pure functions, e.g. Date.now() or Math.random().

```ts
export const ADD_TODO = 'ADD_TODO'
export const TOGGLE_TODO = 'TOGGLE_TODO'
export const SET_VISIBILITY_FILTER = 'SET_VISIBILITY_FILTER'

export const VisibilityFilters = {
  SHOW_ALL: 'SHOW_ALL',
  SHOW_COMPLETED: 'SHOW_COMPLETED',
  SHOW_ACTIVE: 'SHOW_ACTIVE'
}

// Actions

function addTodo(text) {
  return { type: ADD_TODO, text }
}

function toggleTodo(index) {
  return { type: TOGGLE_TODO, index }
}

function setVisibilityFilter(filter) {
  return { type: SET_VISIBILITY_FILTER, filter }
}


// Reducers

const initialState = {
  visibilityFilter: VisibilityFilters.SHOW_ALL,
  todos: []
}

function todoApp(state = initialState, action) {
  switch (action.type) {
    case SET_VISIBILITY_FILTER:
      return Object.assign({}, state, {
        visibilityFilter: action.filter
      })
    default:
      return state
  }
}
```

Note:
- We don't mutate the `state`
- We return the previous state in the default case. It's important to return the previous state for any unknown action.


Spliting Reducers

```ts
function todos(state = [], action) {
  switch (action.type) {
    case ADD_TODO:
      return [
        ...state,
        {
          text: action.text,
          completed: false
        }
      ]
    case TOGGLE_TODO:
      return state.map((todo, index) => {
        if (index === action.index) {
          return Object.assign({}, todo, {
            completed: !todo.completed
          })
        }
        return todo
      })
    default:
      return state
  }
}

function todoApp(state = initialState, action) {
  switch (action.type) {
    case SET_VISIBILITY_FILTER:
      return Object.assign({}, state, {
        visibilityFilter: action.filter
      })
    case ADD_TODO:
    case TOGGLE_TODO:
      return Object.assign({}, state, {
        //todoApp just gives it the slice of the state to manage, and todos knows how to update just that slice. This is called reducer composition, and it's the fundamental pattern of building Redux apps.
        todos: todos(state.todos, action)
      })
    default:
      return state
  }
}
```

In Redux, there is a `combineReducers` that manages different states
```ts
const reducer = combineReducers({
  a: doSomethingWithA,
  b: processB,
  c: c
})
```

### Store

- Holds application state;
- Allows access to state via `getState()`;
- Allows state to be updated via `dispatch(action)`; // 触发state改变的唯一途径
- Registers listeners via `subscribe(listener)`;
- Handles unregistering of listeners via the function returned by subscribe(listener).

Note: We'll have only a single store in a redux application.

we used `combineReducers()` to combine several reducers into one. We will now import it, and pass it to `createStore(reducer)`;

```ts
import { createStore } from 'redux'
import todoApp from './reducers'
let store = createStore(todoApp)
```

#### Dispatching Actions

```ts
import {
  addTodo,
  toggleTodo,
  setVisibilityFilter,
  VisibilityFilters
} from './actions'

// Log the initial state
console.log(store.getState())

// Every time the state changes, log it
// Note that subscribe() returns a function for unregistering the listener
let unsubscribe = store.subscribe(() =>
  console.log(store.getState())
)

// Dispatch some actions
store.dispatch(addTodo('Learn about actions'))
store.dispatch(addTodo('Learn about reducers'))
store.dispatch(addTodo('Learn about store'))
store.dispatch(toggleTodo(0))
store.dispatch(toggleTodo(1))
store.dispatch(setVisibilityFilter(VisibilityFilters.SHOW_COMPLETED))

// Stop listening to state updates
unsubscribe()
```

### Data Flow

The data lifecycle in Redux app follows these 4 steps
- Call `store.dispatch(action)`
- The Redux store calls the reducer function
- The root reducer may combine the output of multiple reducers into a single state tree.  `combineReducers()`
- The Redux store saves the complete state tree returned by the root reducer.


### Conclusion

- store is generated with `createStore(reducer)`
- get state with `store.getState()`
- action is an object with `type` property. It can be generated by a function
- change state by `dispatch` an `action` only
- reducer: update `state` based on `action.type` and return `nextState`

Action Creator => action => store.dispatch(action) => reducer(state, action) => ~~原 state~~ state = nextState



REF:
[ref1](http://www.jianshu.com/p/4407448a1276)
[ref2](http://redux.js.org/docs/basics/UsageWithReact.html)
