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
1. Use __Promise__ to provide data asynchronously from the service

> CONSTRUCTOR:
>
> - A constructor should not contain complex logic (especially a constructor that calls a server). 
> - It is for simple initializations ()like wiring constructor parameters to properties).

> PROMISE:
> 
> - A Promise essentially promises to call back when the results are ready. You ask an asynchronous service to do some work and give it a callback function. The service does that work and eventually calls the function with the results or an error.


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
  promiseHeroes(): Promise<Hero[]> { return Promise.resolve(HEROES); }
  promiseHeroesSlowly(): Promise<Hero[]> {
    return new Promise(resolve => {
      // Simulate server latency with 2 second delay
      setTimeout(() => resolve(this.getHeroes()), 2000);
    });
  }
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

  getHeroes(): void { 
    // this.heroes = this.heroService.getHeroes();  // initial synchronous call
    this.heroService.promiseHeroesSlowly().then(heroes => this.heroes = heroes);
  }

  ngOnInit(): void { this.getHeroes(); }

}

```

- For a multi-word service name, use lower [dash-case](https://angular.io/guide/glossary#dash-case). 
  - For example, the filename for SpecialSuperHeroService is special-super-hero.service.ts.
- [Dependency Injection](https://angular.io/guide/dependency-injection)
- [Lifecycle Hooks](https://angular.io/guide/lifecycle-hooks)
- [Promises for asynchronous programming](http://exploringjs.com/es6/ch_promises.html)
- [Arrow functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions)


## #6 - Routing / navigation

> Routing is another name for navigation. The router is the mechanism for navigating from view to view.

Workflow:

1. Splitting the `AppComponent`
	- Rename it into `HeroesComponent`
	- And include it from new empty `AppComponent`
1. Use the Angular router to enable navigation.
	- The Angular router is an external, optional Angular NgModule called `RouterModule`.
	- The router is a combination of:
		- multiple provided __services__ (`RouterModule`), 
		- multiple __directives__ (`RouterOutlet`, `RouterLink`, `RouterLinkActive`), 
		- and a __configuration__ (`Routes`)
1. Open `index.html` and ensure there is a `<base href="/">` element at the top of the `<head>` section.
1. Configure routes by using `RouterModule.forRoot` in `AppModule` metadata (`@NgModule` decorator)
1. Add the Router outlet
1. Add Router links
1. Create dashboard with route, external HTML template, injection of `HeroService`
1. Crete redirect route for root
1. Navigating to hero details (with parameterized route, add `HeroService.promiseHero()`)
1. Find the way back


### Router outlet

If you paste the path `/heroes` into the browser address bar at the end of the URL, the router should match it to the heroes route and display the `HeroesComponent`. However, you have to tell the router where to display the component.

To do this, you can add a `<router-outlet>` element (one of the directives provided by the `RouterModule`) at the end of the template. The router displays each component immediately below the `<router-outlet>` as users navigate through the app.

- `<router-outlet></router-outlet>`


### Router links

Users shouldn't have to paste a route URL into the address bar. Instead, add an anchor tag to the template that, when clicked, triggers navigation to the `HeroesComponent`: 

- `<a routerLink="/heroes">Heroes</a>`


### `src/app/heroes.component.ts`

Rename the previous `app.component.ts`:

```
@Component({
  selector: 'my-heroes',
  ...
})
export class HeroesComponent implements OnInit {
  ...
}
```

### `src/app/app.component.ts`

```
import { Component } from '@angular/core';

@Component({
  selector: 'my-app',
  template: `
    <h1>{{title}}</h1>
    <!-- <my-heroes></my-heroes> -->
    <a routerLink="/heroes">Heroes</a>
    <router-outlet></router-outlet>
  `
})
export class AppComponent {
  title: 'Tour of Heroes';
}
```

### `src/app/hero-detail.compnent.ts`

```
// Amend the @angular/core import statement to include the 'Input' symbol
import { Component, Input, OnInit } from '@angular/core';
import { ActivatedRoute, ParamMap } from '@angular/router';
import { Location }                 from '@angular/common';

import 'rxjs/add/operator/switchMap';

import { HeroService } from './hero.service';

import { Hero } from './hero';

@Component({
  selector: 'hero-detail',
  templateUrl: './hero-detail.component.html'
})
export class HeroDetailComponent implements OnInit {

  @Input() hero: Hero;

  constructor(
    private heroService: HeroService,
    private route: ActivatedRoute,
    private location: Location
  ) {}

  ngOnInit(): void {
    this.route.paramMap
      .switchMap((params: ParamMap) => this.heroService.promiseHero(+params.get('id')))
      .subscribe(hero => this.hero = hero);
  }

  goBack(): void { this.location.back(); }

}

```

### `src/app/hero.service.ts`

```
...
@Injectable()
export class HeroService {
  promiseHero(id: number): Promise<Hero> {
    return this.getHeroes()
      .then(heroes => heroes.find(hero => hero.id === id));
  }
  ...
}

```

### `src/app/app.module.ts`

```
...
import { RouterModule } from '@angular/router';
...
@NgModule({
  imports: [
    ...
    RouterModule.forRoot([
      {
        path: '',
        redirectTo: 'dashboard',
        pathMatch: 'full'
      },
      {
        path: 'dashboard',
        component: DashboardComponent
      },
      {
        path: 'heroes',
        component: HeroesComponent
      },
      {
        path: 'hero/:id',
        component: HeroDetailComponent
      }
    ])
  ],
  ...
})
export class AppModule { }
```

- [Routing and Navigation](https://angular.io/guide/router):
	- [Appendix: Link Parameters Array](https://angular.io/guide/router#link-parameters-array)
- [CanDeactivate](https://angular.io/api/router/CanDeactivate) to prevent going back too far could take users out of the app