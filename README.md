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
