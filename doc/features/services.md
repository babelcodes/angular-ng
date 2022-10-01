# Services and Dependency Injection

> Class having several responsibilities:
> - Interact with external datasources
> - Share date with other components
> - Independent form other components

- [Tutoriel Angular #24 - Comprendre les services et l'injection des dépendances](https://www.youtube.com/watch?v=dR6dChG2y6c&list=PLrbLGOB571zeR7FUQifKmjUpT4ImldCPt&index=25)
- [Tutoriel Angular #26 - HttpClient & Error Handling avec RxJS](https://www.youtube.com/watch?v=w1PLZ6WptW4&list=PLrbLGOB571zeR7FUQifKmjUpT4ImldCPt&index=27&t=63s)
- See [HTTP Client](./httpclient.md)
- See [RxJS & Observables](./rxjs.md)

The lifecycle is managed by angular:
- through _Dependency Injection_ and the `@Injector` decorator (annotation)
- to get a unique instance of each service in the injector and do not lose resources
- Do not create yourself the service (as `const myService = new MyService();`)
- But give the wanted service as a parameter of the constructor of a component
- There are 2 injectors
  - A **root** injector, available by everybody in the app, and share data
  - A **local** injector per component and their children, to isolate data


## Generate
````shell
$ ng g s hotels
````


## Define
````typescript
import { Injectable } from '@angular/code';   // Import decorator

@Injectable({                                 // Add decorator
  providedIn: 'root'                          // REGISTER (Method #2)
})
export class HotelService {                   // Define class
    
    public getHotels(): Hotel[] {
        return [
          {name: 'H1', price: 123.45, rating: 3.5},  
          {name: 'H2', price: 999, rating: 4},  
        ];
    }
    
}
````


## Register

Register in ROOT injector / App Module (method #1):
````typescript
@NgModule({
  declarations: [],
  imports: [],
  providers: [HotelsService],                 // REGISTER (Method #1)
  bootstrap: [],
})
export class AppModule {}
````


Register in LOCAL injector / Component (method #1):

````typescript
@Component({
  selector: '',
  templateUrl: '',
  styleUrls: [''],
  providers: [HotelsService],                 // REGISTER (Method #1)
})
export class HotelListComponent implements OnInit {

  constructor(
      private hotelsService: HotelsService    // Inject the service in the target component
  ) {
  }

}
````

## HttpClient

- See [HTTP Client](./httpclient.md)
