# Feature Modules

- https://angular.io/guide/feature-modules

A feature module:
* is ___an organizational best practice___, as opposed to a concept of the core Angular API. 
* delivers ___a cohesive set of functionality focused___ on a specific application need such as a user workflow, routing, or forms.
* helps you ___partition the app into focused areas___ while you can do everything within the root module. 
* ___collaborates with the root module and with other modules through___ the services it provides and the components, directives, and pipes that it shares.


Create a feature module using the CLI by entering the following command in the root project directory (you can omit the "Module" suffix from the name because the CLI appends it):

```
ng generate module CustomerDashboard
```

The structure of an NgModule is the same whether it is a root module or a feature module. Feature modules import `CommonModule` instead of `BrowserModule`.

You can now create a component (inside the generated folder `customer-dashboard`):

```
ng generate component customer-dashboard/CustomerDashboard
```

## Code

1/ Import it from root module:

```
// src/app/app.module.ts

// import the feature module here so you can add it to the imports array below
import { CustomerDashboardModule } from './customer-dashboard/customer-dashboard.module';


@NgModule({
  declarations: [...],
  imports: [
    ...
    CustomerDashboardModule // add the feature module here
  ],
  ...
})

export class AppModule { }
```

2/ Export the component from the feature module:

```
// src/app/customer-dashboard/customer-dashboard.module.ts

exports: [
  CustomerDashboardComponent
]
```

3/ Use it in the root component:

```
// src/app/app.component.html


<!-- add the selector from the CustomerDashboardComponent -->
<app-customer-dashboard></app-customer-dashboard>
```
