[Go back to home](https://github.com/babelcodes/angular-ng/tree/master/doc)

# NGRX Store


## Redux: three principles:

1. single source of truth
1. State is read-only
1. Pure functions update state

1\. Single source of truth
 - One state tree inside store
- Predictability maintability
- Universal apps (ssr)
- Testing and debugging

2\. State is read-only
- Derive properties from state
- Dispatch actions to change the state
- Immutable update patterns

3\. Pure functions update state
- Pure functions are reducers
- REducers respond to action types
- Reducers return new state

## Redux: Core Concepts

- Single state tree
- Actions
- Reducers
- Store
- One-way data flow

Single state tree

- Plain javascript object
- Composed by reducers

###Actions

- Two properties
    - Type: string, describes event
    - Payload: optional data
- Dispatch actions to reducers

```
const action = {
  type: ‘ADD_TODO’,
  payload: {.label:’Eat pizza’, complete: false }
}
```

###Reducers

- Pure functions
- Given dispatched action
    - Responds to action.type
    - Access to action.payload
    - Composes new state
    - Returns new state

```
function reducer(state, action) {
  switch(action.type) {
    Case ‘ADD_TODO’: {
      const toto = action.payload;
      const totos = […state.todos, todo];
    }
  }
  return state;
}
```

```
const state = {
  todos: [
    {.label:’Eat pizza’, complete: false }
  ]
}
```

###Store

- State container
- Components interact with the Store
    - Subscribe to slice of State
    - Dispatch Actions to the Store
- Store invokes Reducers with previous State and Action
- Reducers compose new State
- Store is updated, notifies subscribers

###One-way data flow

```
                       ┏ ━ ━ ━ ━ ━ ━ ━ ━ ━ ━ ━ ━ ━ ━ ━ ━ ━ ━ ━ ━ ━ ━ ━ ┓
                                                                        
                       ┃                                               ┃
                             ┌─────────────┐                            
       ┌───────────────╋────▶│   Action    │───────────────┐           ┃
       │   Dispatch          └─────────────┘    Sent to    │            
       │               ┃                                   │           ┃
       │                                                   │            
       │               ┃                                   │           ┃
       │                                                   │            
       │               ┃                                   │           ┃
       │                                                   │            
       │               ┃                                   ▼           ┃
┌─────────────┐                                     ┌─────────────┐     
│  Component  │        ┃      STORE                 │   Reducer   │    ┃
└─────────────┘                                     └─────────────┘     
       ▲               ┃                                   │           ┃
       │                                                   │            
       │               ┃                                   │           ┃
       │                                                   │            
       │               ┃                                   │           ┃
       │                                                   │            
       │               ┃                                   │           ┃
       │   Render            ┌─────────────┐   New State   │            
       └───────────────╋─────│    State    │◀──────────────┘           ┃
                             └─────────────┘                            
                       ┃                                               ┃
                                                                        
                       ┗ ━ ━ ━ ━ ━ ━ ━ ━ ━ ━ ━ ━ ━ ━ ━ ━ ━ ━ ━ ━ ━ ━ ━ ┛

```

## Immutability

> « An immutable object is an object whose state cannot be modified after creation »

Implies (update) copy

Why ?
- Predictability
- Explicit state changes
- Performance (Change detection)
- Mutation tracking
- Undo state changes
