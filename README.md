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