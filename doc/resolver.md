[Go back to home](https://github.com/babelcodes/angular-ng/tree/master/doc)

# Resolver

- https://angular.io/api/router/Resolve
- [Understanding Resolvers in Angular. – codeburst](https://codeburst.io/understanding-resolvers-in-angular-736e9db71267)

> So basically `resolver` is that intermediate code, which can be executed when a link has been clicked and before 
> a component is loaded.


Routing Flow with Resolver:

1. User clicks the link.
1. Angular executes certain code and returns a value or observable.
1. You can collect the returned value or observable in constructor or in ngOnInit, in class of your component which is about to load.
1. Use the collected the data for your purpose.
1. Now you can load your component.

Steps 2,3 and 4 are done with a code called Resolver.


Codding flow: 

1. [Create a service](#my.resolver.ts), aka the resolver
   1. Decorate qith `@Injecable()`
   1. Implements `Resolve<T>` interface from `@angular/router`
   1. Override `resolve(route: ActivatedRouteSnapshot, state: RouterStateSnapshot): Observable<T>`
1. Complete routes
    1. [The router](#app.routes.ts) `app.routes.ts`
    1. [The module](#my.module.ts) `my.module.ts`
1. [Use it](#some.component.ts) in `some.component.ts`    


## my.resolver.ts

```
import { Injectable } from '@angular/core';
import { Observable } from 'rxjs/Observable';
import { Resolve, ActivatedRouteSnapshot } from '@angular/router';
import 'rxjs/add/operator/map';
import 'rxjs/add/operator/catch';

import { MyModel } from '../shared/models/my.modele';
import { MyService } from '../shared/services/my.service';

@Injectable()
export class MyResolver implements Resolve<MyModel> {
  constructor(private myService: MyService) {}

  resolve(route: ActivatedRouteSnapshot): Observable<MyModel> {
    return this.myService.getMyList();
  }
}
```

## app.routes.ts

```
...
import { MyResolver } from './resolvers/my.resolver';

export const ROUTES: Route[] = [
  {
    path: 'bases/:base_id',
    resolve: {
      myResolver: MyResolver,
    },
    children: [
      { path: '', pathMatch: 'full', redirectTo: 'grid' },
      {
        path: 'dashboard',
        component: DashboardComponent,
      },
      {
        path: 'grid',
        component: PartsBaseComponent,
      },
    ],
  },
];
```


## my.module.ts

```
...
import { MyResolver } from '../../resolvers/my.resolver';

@NgModule({
  imports: [...],
  exports: [...],
  declarations: [...],
  providers: [
    MyResolver,
    ...
  ],
  entryComponents: [...],
})
export class MyModule {}
```


## some.component.ts

```
...
import { ActivatedRoute } from '@angular/router';
import { MyModel } from 'app/shared/models/my.model';

@Component({
  selector: 'some',
  templateUrl: './some.component.html',
  styleUrls: ['./some.component.scss'],
})
export class SomeComponent implements OnInit {

  myModel: MyModel;

  constructor(private route: ActivatedRoute) {}

  ngOnInit(): void {
    // this.route.data.map(data => data['myResolver']).subscribe((res) => this.myModel = res);
    this.myModel = this.route.snapshot.data['myResolver'];
  }

  ...
}
```
