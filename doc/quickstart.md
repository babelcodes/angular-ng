# Quickstart

```
cd /Users/jacques/Dropbox/-work/projects/-sources/js/angular/
git clone https://github.com/angular/quickstart.git quickstart
cd quickstart
npm install
npm start
```

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

## Material Design

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
