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

- [Displaying Data](https://angular.io/guide/displaying-data)
