# Resources

```
cd /Users/jacques/Dropbox/-work/projects/-sources/js/angular/
git clone https://github.com/angular/quickstart.git quickstart
cd quickstart
npm install
npm start
```

## Resources

- [style guide](https://angular.io/guide/styleguide#rule-of-one)
- [dependency injection](https://angular.io/guide/dependency-injection)
- [lifecycle hooks](https://angular.io/guide/lifecycle-hooks)
- [Promises](http://exploringjs.com/es6/ch_promises.html) - _A Promise essentially promises to call back when the results are ready_ / act on the Promise when it resolves
- [Arrow functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions)
- [Routing and Navigation](https://angular.io/guide/router)
- https://angular.io/api/router/CanDeactivate

## Courses

- [Todd Motto](https://platform.ultimateangular.com/courses/)




# ng2

- [angular.io](https://angular.io)
  - [/styleguide](https://angular.io/styleguide)

## Install

### Angular

- https://github.com/angular/angular-cli

```
node -v     ### 7.x
npm install -g @angular/cli
```

## Create new project

```
ng new MyProject
cd MyProject
ng serve
open http://localhost:4200
```

=> 235 Mo.

- `tsconfig.json` informations for the compiler __codelyzer__
- `tslint.json`... apply by `codelyzer`

### Material Design

- [material.angular.io](https://material.angular.io/)
  - [material.io/guidelines](https://material.io/guidelines/)

```
cd MyProject
npm install --save @angular/material
```

Add link to the font in your `/src/index.html`:

```
<link href="https://fonts.googleapis.com/css?family=Roboto:300,400,700|Material+Icons" rel="stylesheet">
```


## Develop

- `.angular-cli.json`
  - `/src/main.ts`
  - `/src/app/app.module.ts` // tirer les dépendances
  - `/src/app/app.component.ts`


### Module

TODO

### Component

```
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  title = 'app works!';
}
```

My new component:

```
cd src/app
ng g component /components/project
ng g component /components/task
```

Add it to an HTML page:

```
    <app-project></app-project>
    <app-task></app-task>
```


### Router

TODO.


### Service

```
ng g service components/project/services/project
ng g module components/project
```


## Firebase

- https://firebase.google.com
  - > Console
  - > Create New Project

```
npm install --save angularfire2 firebase
```

- `firebase` is the low level API
- But we will use the highest level API called `angularfire2` through `AngularFireModule`

### Connection

- Create `FIREBASE_CONFIG`
- Inject it from `app.module` with `AngularFireModule`
- Update Firebase rules (in web console) to be able to READ/WRITE entities

### READ

- Create a Service who reads observables (`xxx$`) from `angularfire` API
- Provide this Service to the Component from the Module
- Inject this Service in the Component constructor
- Display items in the HTML view

--
--
--

# Concepts

- Component
- Directive
- Pipe `{{ myVar | json }}`
- Service
- Class
- Interface
- Enum
- Module


## Syntax

Databinding:

- `[...]` to send value:

```
<md-list-item [routerLink]="['projects']">Projects</md-list-item>
```

- `(...)` to receive value:

```
    <button md-icon-button (click)="start.toggle()"><md-icon>menu</md-icon></button>
```


## Components

* Smart / Container component
  * Communicate with services
  * Render child components
* Dumb / Presentational component
  * Accept data via inputs
  * Emit data changes via event outputs

One-way dataflow:
- data flows down
- events emit up


## Feature Modules

See [Feature Modules page](feature-module.md)

## Dumb / Presentational component

See [Presentational component page](presentational-component.md)
