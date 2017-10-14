# Tutorial: Tour of Heroes

- [https://angular.io/tutorial/toh-pt1](https://angular.io/tutorial/toh-pt1)

## #1 - Setup

```
git clone https://github.com/angular/quickstart.git angular2-tour-of-heroes
cd angular2-tour-of-heroes
npm install
npm start

xargs rm -rf < non-essential-files.osx.txt
rm src/app/*.spec*.ts
rm non-essential-files.osx.txt

git init
```

Get `.gitignore` file from [https://github.com/angular/quickstart/blob/master/.gitignore](https://github.com/angular/quickstart/blob/master/.gitignore).

Main files:

```
src
|___ app
| |___ app.component.ts		// the root component
| |___ app.module.ts		// the root module. Right now it declares only the AppComponent
|___ main.ts
```

- [The root module](https://angular.io/guide/bootstrapping)


## #2 - The Hero Editor


### `src/app/app.component.ts`

```
...

@Component({
  ...
  template: `
    ...
      <input [(ngModel)]="hero.name" placeholder="name">
    ...
  `
})
export class AppComponent  {
  ...
}
```

### `app.module.ts`

```
...
import { FormsModule }   from '@angular/forms'; // <-- NgModel lives here

...

@NgModule({
  imports:      [
    ...
    FormsModule // <-- import the FormsModule before binding with [(ngModel)]
  ],
  ...
})
export class AppModule { }

```

- [Displaying Data](https://angular.io/guide/displaying-data)
- [Two-way data binding with ngModel](https://angular.io/guide/forms#ngModel)
- [Forms](https://angular.io/guide/forms)
- [Two-way binding with NgModel](https://angular.io/guide/template-syntax#ngModel)
- [Template Syntax](https://angular.io/guide/template-syntax)


## #3 - Master / Detail

The `(*)` prefix to `ngFor` is a critical part of this syntax. It indicates that the `<li>` element and its children constitute a master template.


```
...
@Component({
  selector: 'my-app',
  template: `
    ...
    <ul class="heroes">
      <li *ngFor="let hero of heroes"
          [class.selected]="hero === selectedHero"
          (click)="onSelect(hero)">
          <!-- The parentheses identify the <li> element's click event as the target -->
        <span class="badge">{{hero.id}}</span> {{hero.name}}
      </li>
    </ul>
    <div *ngIf="selectedHero">
      <h2>{{selectedHero.name}} details!</h2>
      ...
    </div>
    ...
  `,
  styles: [`
    .selected {
      background-color: #CFD8DC !important;
      color: white;
    }
    ...
  `]
})
export class AppComponent  {
  ...
  heroes = HEROES;
}
```

- [Showing an array property with *ngFor](https://angular.io/guide/displaying-data#ngFor)
- [User Input](https://angular.io/guide/user-input)
- [Event binding](https://angular.io/guide/template-syntax#event-binding)
- [Template Syntax](https://angular.io/guide/template-syntax)
- [Structural Directives](https://angular.io/guide/structural-directives)
- [Built-in directives](https://angular.io/guide/template-syntax#directives)


## #4 - Multiple Components


### `src/app/app.component.ts`
```
...

@Component({
  ...
  template: `
    ...
    <hero-detail [hero]="selectedHero"></hero-detail>
  `,
  ...
})
export class AppComponent  {
  ...
}
```

### `src/app/hero-detail.component.ts`

```
// Amend the @angular/core import statement to include the 'Input' symbol
import { Component, Input } from '@angular/core';

import { Hero } from './hero';

@Component({
  selector: 'hero-detail',
  template: `
    <div *ngIf="hero">
      ...
        <input [(ngModel)]="hero.name" placeholder="name"/>
      ...
    </div>
  `
})
export class HeroDetailComponent {

  @Input() hero: Hero;

}
```

### `src/app/app.module.ts`


```
...
import { HeroDetailComponent } from './hero-detail.component';

@NgModule({
  imports:      [
    BrowserModule,
    FormsModule // <-- import the FormsModule before binding with [(ngModel)]
  ],
  declarations: [
    AppComponent,
    HeroDetailComponent
  ],
  bootstrap:    [ AppComponent ]
})
export class AppModule { }
```

- [Attribute Directives](https://angular.io/guide/attribute-directives#why-input) - Read more about `input` properties
- [NgModules](https://angular.io/guide/ngmodule)


## #5 - Services

- Import the Angular Injectable function and apply that function as an @Injectable() decorator.
- The `@Injectable()` decorator tells TypeScript to emit metadata about the service.
- The metadata specifies that Angular may need to inject other dependencies into this service.

Workflow:

1. Create a __Mock__ to host data (move `HEROES` constant inside and export it)
1. Create a __Service__ to provide data (via included mock)
1. __Inject__ service in the __Component__ via:
	1.  `providers` property of component metadata 
	1. and constructor `private` property)
1. __Load__ data from the service
	1. By implementing the Angular ngOnInit lifecycle hook

> WARNING:
>
> - A constructor should not contain complex logic (especially a constructor that calls a server). 
> - It is for simple initializations ()like wiring constructor parameters to properties).

### `src/app/hero.service.ts`

```
// Import the Angular Injectable function and apply that function as an @Injectable() decorator.
// The @Injectable() decorator tells TypeScript to emit metadata about the service.
// The metadata specifies that Angular may need to inject other dependencies into this service.
import { Injectable } from '@angular/core';

import { HEROES } from './mock-heroes';
import { Hero } from './hero';

@Injectable()
export class HeroService {
  getHeroes(): Hero[] {  HEROES; }
}
```

### `src/app/app.component.ts`

```
...
import { OnInit } from '@angular/core';
import { HeroService } from './hero.service';

// The providers array tells Angular to create a fresh instance of the HeroService when it creates an AppComponent.
// The AppComponent, as well as its child components, can use that service to get hero data.
@Component({
  selector: 'my-app',
  providers: [ HeroService ],
  ...
})
export class AppComponent implements OnInit {

  ...

  // The constructor itself does nothing.
  // The parameter simultaneously defines a private heroService property and identifies it as a HeroService injection site.
  constructor(private heroService: HeroService) {}

  getHeroes(): void { this.heroes = this.heroService.getHeroes(); }

  ngOnInit(): void { this.getHeroes(); }

}

```

- For a multi-word service name, use lower [dash-case](https://angular.io/guide/glossary#dash-case). 
  - For example, the filename for SpecialSuperHeroService is special-super-hero.service.ts.
- [Dependency Injection](https://angular.io/guide/dependency-injection)
- [Lifecycle Hooks](https://angular.io/guide/lifecycle-hooks)
