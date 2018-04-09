[Go back to home](https://github.com/babelcodes/angular-ng/tree/master/doc)

# NGRX Store

## Ressources

- [Monorepo for ngrx codebase](https://github.com/ngrx/platform)
- [A Comprehensive Introduction to @ngrx/store - Companion to Egghead.io Series](https://gist.github.com/btroncone/a6e4347326749f938510)

### Videos

- [NGRX store/effects | Ultimate Angular](https://platform.ultimateangular.com/courses/enrolled/227301) from Todd Motto
  - [https://github.com/UltimateAngular/ngrx-store-effects-app](https://github.com/UltimateAngular/ngrx-store-effects-app)
- [@ngrx/store in 10 minutes from @btroncone on @eggheadio](https://egghead.io/lessons/angular-2-ngrx-store-in-10-minutes)

### Tools

- [Redux DevTools - Chrome Web Store](https://chrome.google.com/webstore/detail/redux-devtools/lmhkpmbekcpmknklioeibfkpmmfibljd/related?hl=fr)



## Summary

- `Store<T>` 
  - (where `T` is a `State`, see `TdProjectState` example bellow)
  - Single source of truth / immutable State
  - "in-memory database"
- `Action`
  - a `type`
  - a `payload`
- `Reducer`
  - Pure function taking:
     - Previous state
     - An Action
- `select` as Selector
  - different properties at different levels
  - separate applications state with component trees
  - compose sates 
  - slice we need to a particular component
- `Effect`
  - ngrx/effects



```
export interface TdProjectState {
  errors: fromTdProject.TdError;
}
```

### Folders

```
.
|____store
| |____actions
| | |____pizza.action.ts
| |____reducers
| | |____index.ts
| | |____pizza.reducer.ts
| |____index.ts
```



## Redux: 3 principles

- [Redux DevTools - Chrome Web Store](https://chrome.google.com/webstore/detail/redux-devtools/lmhkpmbekcpmknklioeibfkpmmfibljd/related?hl=fr)

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
  type: 'ADD_TODO',
  payload: { label:'Eat pizza', complete: false }
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
    Case 'ADD_TODO': {
      const toto = action.payload;
      const totos = [...state.todos, todo];
    }
  }
  return state;
}
```

```
const state = {
  todos: [
    { label:'Eat pizza', complete: false }
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


## Actions

```
// myFeatureComponent/store/actions/pizza.action.ts

import { Action } from '@ngrx/store';
import { Pizza } from '../../models/pizza.model';

// Load pizzas
export const LOAD_PIZZAS = '[Products] Load Pizzas';
export const LOAD_PIZZAS_FAIL = '[Products] Load Pizzas Fail';
export const LOAD_PIZZAS_SUCCESS = '[Products] Load Pizzas Success';

export class LoadPizzas implements Action {
  readonly type = LOAD_PIZZAS;
  
}

export class LoadPizzasFail implements Action {
  readonly type = LOAD_PIZZAS_FAIL;
  constructor(public payload: any) {}
}

export class LoadPizzasFail implements Action {
  readonly type = LOAD_PIZZAS_SUCCESS;
  constructor(public payload: Pizza[]) {}
}

// Action types
export type PizzasAction = LoadPizzas | LoadPizzasFail | LoadPizzasSuccess;
```


## Reducer

```
// myFeatureComponent/store/reducers/pizza.reducer.ts

import { * } as fromPizzas from '../actions/pizzas.action';
import { Pizza } from '../../models/pizza.model';

export interface PizzaState {
  data: Pizza[];
  loaded: boolean;
  loading: boolean;
}

export const initialState :PizzaState = {
  data: [],
  loaded: false,
  loading: false
};

export function reducer(state = initialState, action: fromPizzas.PizzasAction): PizzaState {

  switch(action.type) {
    case fromPizzas.LOAD_PIZZAS: {
      return {
        ...state,
        loading: true
      };
    }
    case fromPizzas.LOAD_PIZZAS_SUCCESS: {
      return {
        ...state,
        loading: false,
        loaded: true
      };
    }
    case fromPizzas.LOAD_PIZZAS_FAIL: {
      return {
        ...state,
        loading: false,
        loaded: false
      };
    }
  }

  return state;
}

```


```
// myFeatureComponent/store/reducers/index.ts

import { ActionReducerMap } from '@ngrx/store';
import { * } as fromPizzas from './pizzas.reducer';

export interface ProductsState {
  pizzas: fromPizzas.PizzaState;
}

export const reducers :ActionReducerMap<ProductsState> = {
  pizzas: fromPizzas.reducer;
};
```

```
// myFeatureComponent/store/index.ts

export * from './reducers';
```

```
// myFeatureComponent/myFeatureComponent.module.ts

import { StoreModule } from '@ngrx/store';

import { reducers } from './store';

@NgModule ... imports: ... StoreModule.forFeature('products', reducers)
```

### Selectors

```
// myFeatureComponent/containers/products/products.component.ts

import { Store } from '@ngrx/store';
import { Observable } from '@ngrx/Observable';

import * as fromStore from '../../store';
import { Pizza } from '../../models/pizza.model';

...

export class ProductsComponent implements OnInit {

  constructor(private store. Store<fromStore.ProductsState>) {}

  ngOnInit() {
    this.store.select<any>('products').subscribe(state => console.log(state); );
    // TODO...
  }

}
```

```
// myFeatureComponent/store/reducers/pizza.reducer.ts

...

export const getPizzasLoading = (state: PizzaState) => state.loading;
export const getPizzasLoaded = (state: PizzaState) => state.loaded;
export const getPizzas = (state: PizzaState) => state.data;
```

```
// myFeatureComponent/store/reducers/index.ts

import { ActionReducerMap, createSelector, createFeatureSelector } from '@ngrx/store';

...

export const getProductsState = createFeatureSelector<ProductState>('products');

// Selectors

export const getPizzaState = createSelector(/*starting from...*/ getProductsState, (state: ProductsState) => state.pizzas);

export const getAllPizzas = createSelector(getPizzaState, fromPizzas.getPizzas);
export const getPizzasLoaded = createSelector(getPizzaState, fromPizzas.getPizzasLoaded);
export const getPizzasLoading = createSelector(getPizzaState, fromPizzas.getPizzasLoading);
```

```
const state = {
  products: {
    pizzas: {
      data: [],
      loaded: false,
      loading: false
    }
  }
}
```

```
// myFeatureComponent/containers/products/products.component.ts

import { Store } from '@ngrx/store';
import { Observable } from '@ngrx/Observable';

import * as fromStore from '../../store';
import { Pizza } from '../../models/pizza.model';

...

export class ProductsComponent implements OnInit {

  pizzas$: Observable<Pizza[]>;

  constructor(private store. Store<fromStore.ProductsState>) {}

  ngOnInit() {
    // this.store.select<any>(fromStore.getAllPizzas).subscribe( state => console.log(state); );
    this.pizzas$ = this.store.select(fromStore.getAllPizzas);
  }

}
```


## Effects

ngrx/effects

- Listen for ngrx/store actions
- Isolate side effects from components
- Communicate outside of Angular

Effect is a pure function


### Effect flow

Completing the "One-way data flow" viewed before:

```
                                One-way data flow                                                  Effects flow        
                                =================                                                  ============        
                                                                                                                       
                                                                                                                       
                                                                                                                       
                                                                                                                       
                                                                                                                       
                       ┏ ━ ━ ━ ━ ━ ━ ━ ━ ━ ━ ━ ━ ━ ━ ━ ━ ━ ━ ━ ━ ━ ━ ━ ━ ━ ━ ━ ━ ━ ━ ━ ━ ━ ┓  ┏ ━ ━ ━ ━ ━ ━ ━ ━ ━ ━ ┓  
                                                       STORE                                          EFFECTS          
                       ┃                                                                   ┃  ┃                     ┃  
                             ┌─────────────┐                                                                           
       ┌───────────────╋────▶│   ACTION    │───────────────┬───────────────────────────────╋──╋──────────┐          ┃  
       │   Dispatch          └─────────────┘    Sent to    │                                             │             
       │               ┃                                   │                               ┃  ┃          │          ┃  
       │                                                   │                                             │             
       │               ┃                                   │                               ┃  ┃          │          ┃  
       │                                                   │                                             │             
       │               ┃                                   │                               ┃  ┃          │          ┃  
       │                                                   │                                             │             
       │               ┃                                   ▼                               ┃  ┃          ▼          ┃  
┌─────────────┐                                     ┌─────────────┐     ┌─────────────┐           ┌─────────────┐      
│  COMPONENT  │        ┃                            │   REDUCER   │◀────│   ACTION    │◀───╋──╋───│   EFFECT    │   ┃  
└─────────────┘                                     └─────────────┘     └─────────────┘           └─────────────┘      
       ▲               ┃                                   │                               ┃  ┃      ▲   │          ┃  
       │                                                   │                                         │   │             
       │               ┃                                   │                               ┃  ┃      │   ▼          ┃  
       │                                                   │                                    ┌─────────────┐        
       │               ┃                                   │                               ┃  ┃ │   Service   │     ┃  
       │                                                   │                                    └─────────────┘        
       │               ┃                                   │                               ┃  ┃      ▲   │          ┃  
       │   Render            ┌─────────────┐   New state   │                                         │   │             
       └───────────────╋─────│    STATE    │◀──────────────┘                               ┃  ┃      │   ▼          ┃  
                             └─────────────┘                                                    ┌─────────────┐        
                       ┃                                                                   ┃  ┃ │   Server    │     ┃  
                                                                                                └─────────────┘        
                       ┗ ━ ━ ━ ━ ━ ━ ━ ━ ━ ━ ━ ━ ━ ━ ━ ━ ━ ━ ━ ━ ━ ━ ━ ━ ━ ━ ━ ━ ━ ━ ━ ━ ━ ┛  ┗ ━ ━ ━ ━ ━ ━ ━ ━ ━ ━ ┛  
```