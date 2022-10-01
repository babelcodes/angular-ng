# HTTP Client

- [Services and Dependency Injection](./services.md)
- [RxJS & Observables](./rxjs.md)
- [Tutoriel Angular #26 - HttpClient & Error Handling avec RxJS](https://www.youtube.com/watch?v=w1PLZ6WptW4&list=PLrbLGOB571zeR7FUQifKmjUpT4ImldCPt&index=27&t=63s)

```
    ┌──────────────────┐ GET                           ┌──────────────────┐ GET                           ┌──────────────────┐
    │                  │ https://api.com/hotels        │                  │ https://api.com/hotels        │                  │
    │                  ├──────────────────────────────►│                  ├──────────────────────────────►│                  │
    │      Hotel       │                               │       Http       │                               │                  │
    │     Service      │                               │      Service     │                               │      Server      │
    │                  │                               │   (HttpClient)   │                               │                  │
    │                  │◄──────────────────────────────┤                  │◄──────────────────────────────┤                  │
    │                  │ HTTP RESPONSE                 │                  │ HTTP RESPONSE                 │                  │
    └──────────────────┘ (Observable)                  └──────────────────┘                               └──────────────────┘
```

## Generate
````shell
$ ng g s hotels
````


## Define
````typescript
import { HttpClient } from '@angular/common/http';
import { Injectable } from '@angular/code';

@Injectable({
  providedIn: 'root'
})
export class HotelService {
    
    private readonly API_URL = 'api/hotels.json';          // Define the DATASOURCE (A URL in the real life)

    constructor(
      private http: HttpClient                             // INJECT service
    ) {
    }

    public getHotels(): Observable<Hotel[]> {
        return this.http.get<Hotel[]>(this.API_URL);       // SEND the request
    }
    
}
````


## Register

Register in ROOT injector / App Module (method #1):
````typescript
import { HttpClientModule } from '@angular/common/http';

@NgModule({
  declarations: [],
  imports: [HttpClientModule],                             // REGISTER the HttpClient service (from the associated module created by angular)
  providers: [],
  bootstrap: [],
})
export class AppModule {}
````


## Subscribe

```typescript
// Observable.subscribe(observer /* {next, error, complete} */)

import { ActivatedRoute } from '@angular/router';

export class HodelListComponent implements OnInit {

    private hotels: Hotel[];
    
    constructor(
      private hotelService: HotelService,
    ) {}

    ngOnInit() {
        this.hotelService.getHotels().subscribe({
            next: hotels => {
                this.hotels = hotels;
                this.filteredHotels = this.filterHotels(hotels);
            },
            error: error => this.error = error,
        });
    }
  
}
```


## Extends with Observables

Use the power of Observables to handle errors on API:
```typescript
import { HttpClient, HttpErrorResponse } from '@angular/common/http';
import { Observable, throwError } from 'rxjs';
import { tap, catchError } from 'rxjs/oeprators';

export class HotelService {
    
    public getHotels(): Observable<Hotel[]> {
        return this.http.get<Hotel[]>(this.API_URL).pipe(
            tape(hotels => console.log('Hotels:', hotels)),
            catchError(this.handleError)
        );
    }

    // https://angular.io/guide/http#getting-error-details
    private handleError(error: HttpErrorResponse) {
      if (error.status === 0) {
        // A client-side or network error occurred. Handle it accordingly.
        console.error('An error occurred:', error.error);
      } else {
        // The backend returned an unsuccessful response code.
        // The response body may contain clues as to what went wrong.
        console.error(
          `Backend returned code ${error.status}, body was: `, error.error);
      }
      // Return an observable with a user-facing error message.
      return throwError(() => new Error('Something bad happened; please try again later.'));
    }
    
}
```


## Local Resource

To allow angular HTTP Client to access local resource (aka `api/hotels.json`) we need to update configuration in `/angular.json`:

```json
{
  "projects": {
    "my-app": {
      "architect": {
        "build": {
          "options": {
            "assets": [
              ...
              "src/api"
            ],
          },
        },
      }
    },
  },
}
```
