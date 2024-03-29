## Table of content

- [Tree](#tree)
- [Syntax](#syntax)
  - Feature Modules
- [Component](#component)
  - Smart / Container or Dumb / Presentational 
- [Resolver](#resolver)
- Interceptor
- Directive
- Pipe `{{ myVar | json }}`
- [Typescript](http://babel.codes/TypeScript/index)
  - Class
  - Interface
  - Enum

Advanced:

- [NGRX Store](ngrx-store.md)

See also:

- [Firebase](firebase.md)
- [Quickstart](quickstart.md)

## Tree

- `.angular-cli.json`
  - `/src/main.ts`
  - `/src/app/app.module.ts` // tirer les dépendances
  - `/src/app/app.component.ts`


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


## Component

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

* Smart / Container component
  * Communicate with services
  * Render child components
* [Dumb / Presentational component](presentational-component.md)
  * Accept data via inputs
  * Emit data changes via event outputs

One-way dataflow:
- data flows down
- events emit up


## Resolver

See [Resolver page](/resolver.md)
