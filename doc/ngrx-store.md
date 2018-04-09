[Go back to home](https://github.com/babelcodes/angular-ng/tree/master/doc)

# NGRX Store


## Redux: 3 principles

### 1\. Single source of truth
 - One state tree inside store
- Predictability maintability
- Universal apps (ssr)
- Testing and debugging

### 2\. State is read-only
- Derive properties from state
- Dispatch actions to change the state
- Immutable update patterns

### 3\. Pure functions update state
- Pure functions are reducers
- REducers respond to action types
- Reducers return new state

## Redux: Core Concepts

- [Single state tree](#single-state-tree)
- [Actions](#actions)
- [Reducers](#reducers)
- [Store](#store)
- [One-way data flow](#one-way-data-flow)

### Single state tree

- Plain javascript object
- Composed by reducers

### Actions

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

### Reducers

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

### Store

- State container
- Components interact with the Store
    - Subscribe to slice of State
    - Dispatch Actions to the Store
- Store invokes Reducers with previous State and Action
- Reducers compose new State
- Store is updated, notifies subscribers

### One-way data flow

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

Implies (use / update) copy

Why ?
- Predictability
- Explicit state changes
- Performance (Change detection)
- Mutation tracking
- Undo state changes

### Mutable in JavaScript (by design)

Object:

```
const character = { name: 'Han Solo' };
character.role = 'Captain';

console.log(character); // ==> { name: 'Han Solo', role: 'Captain' }
```

Array:

```
const names = ['Han Solo', 'Darth Vader'];
names.push('R2-D2');

console.log(names); // ==> ['Han Solo', 'Darth Vader', 'R2-D2']
```


### Immutability in JavaScript (by design)

Immutable by design (String):

```
const name = 'Han Solo';
const uppercaseName = name.toUpperCase();

console.log(name, uppercaseName); // ==> 'Han Solo', 'HAN SOLO'
```

Immutable operations (Object):

```
const character = { name: 'Han Solo' };

// Object.assign({}, character, { role: 'Captain' });
const updatedCharacter = { ...character, role: 'Captain' };

console.log(character); // ==> { name: 'Han Solo' }
console.log(updatedCharacter); // ==> { name: 'Han Solo', role: 'Captain' }
```

Immutable operations (Array):

```
const names = ['Han Solo', 'Darth Vader'];
const newNames = [...names, 'R2-D2'];

console.log(names); // ==> ['Han Solo', 'Darth Vader']

console.log(newNames); // ==> ['Han Solo', 'Darth Vader', 'R2-D2']
```

## ngrx/store

- Based on Redux
- Written with Observables
- Made for Angular

### Benefits

- Single source of truth
- Testability
- Performance benefits
  - `ChangeDetectionStrategy.OnPush`
  - Immutable `@Inputs`
  - Object reference checks are fast
- Root and feature module support
  - Eagerly loaded modules
  - Lazily loaded modules

Container

- Aware of Store
- Dispatches Actions
- Reads data from Store

Presentational 

- Not awake of Store
- Invokes callbacks via `@Output`
- Read data from `@Inputs` 

### Reactive Angular

```
                             ┌─────────┐                        
                             │         │                        
                             │  Store  │                        
                             │         │                        
                             └─────────┘                        
                                                                
                               ▲    │                           
                                    │                           
                    Dispatch   │    │    Select                 
                                    │                           
                               │    ▼                           
                      ┌───────────────────────┐                 
                      │                       │                 
                      │       Container       │                 
                      │       Component       │                 
                      │                       │                 
                      └───────────────────────┘                 
                        ▲  │             ▲  │                   
                           │                │                   
              @Ouput    │  │             │  │    @Input         
                           │                │                   
                        │  ▼             │  ▼                   
      ┌───────────────────────┐        ┌───────────────────────┐
      │                       │        │                       │
      │       Presenta.       │        │       Presenta.       │
      │       Component       │        │       Component       │
      │                       │        │                       │
      └───────────────────────┘        └───────────────────────┘
```
