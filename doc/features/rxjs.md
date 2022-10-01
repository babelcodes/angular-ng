[Go back to home](https://github.com/babelcodes/angular-ng/tree/master/doc)

# RxJS & Observables

> Reactive Extensions Library for JavaScript
> Manage events arriving in an asynchronous way.

- https://rxjs.dev
- [Tutoriel Angular #25 - RxJS et les Observables + exemples dynamiques](https://www.youtube.com/watch?v=aZcPIkSKHv4&list=PLrbLGOB571zeR7FUQifKmjUpT4ImldCPt&index=26&t=5s)
- [Tutoriel Angular #26 - HttpClient & Error Handling avec RxJS](https://www.youtube.com/watch?v=w1PLZ6WptW4&list=PLrbLGOB571zeR7FUQifKmjUpT4ImldCPt&index=27&t=63s)


## Get started

- The packages `Http`, `Forms` and `Router` are built on RxJS
- `Observable.subscribe()` may provide several values (mutable) vs. `Promise.then()` that provide a single value (immutable).


## Reference

### Observable

- You need to `subscribe()` and maybe to `unsubscribe()`
- `operators` handle `observables`:
  - map, filter, concat, find, take...
- Usage and composition with `pipe()` 
- Two packages: `rxjs` and `rxjs/operators`

```typescript
import {
  Observable
} from 'doc/features/rxjs';

const stream = new Observable(observer => {
  for (var i = 0; i < 10; i++) {
    observer.next(`Event #${i}`);
  }
  observer.complete();
});

const subscription = stream.subscribe(
  item => console.log(`Un item arrrive: ${item}`),
  error => console.error(`Une erreur: ${error}`),
  () => console.log(`ZE END!`),
);

subscription.unsubscribe();
```

### Sample

````typescript
import { Observable, range } from 'rxjs';
import { map, filter } from 'rxjs/operators';

const source$: Observable<number> = range(0, 10);
source$.pipe(
    map(i => i * 3),
    filter(i => i % 2 === 0)
).subscribe(console.log);
````


### Subscriptions

```typescript
import {
  Observable,
  interval,
  Subscription
} from 'doc/features/rxjs';

const start = () => interval(1000).subscribe(
  item => console.log(`Valeur = ${item}`),
  error => console.error(`Une erreur: ${error}`),
  () => console.log(`ZE END!`),
);

const subscription1 = start();
const subscription2 = start();
subscription1.unsubscribe();
subscription2.unsubscribe();

const subscriptions: Subscription[] = [start()];
subscriptions.push(start());
subscriptions.forEach(subscription => subscription.unsubscribe())

// USE Subscription TO REMOVE ARRAYS
// IN ngOnDestroy() TO PREVENT MEMORY LEAKS
const subscription = new Subscription();
subscription.add(start());
subscription.add(start());
subscription.unsubscribe();
```

### Creation

of vs. from

```typescript
import {
  Observable,
  interval,
  Subscription,
  of
} from 'doc/features/rxjs';

const streamOf = of('Event #1', 'Event #2', 'Event #3'); // COMPLETE AUTOMATICALLY
const streamFrom = from(['Event #1', 'Event #2', 'Event #3']); // IDEM

const events = ['Event #1', 'Event #2', 'Event #3'];

of(events).subscribe(console.log); // => ONE VALUE = ['Event #1', 'Event #2', 'Event #3']
from(events).subscribe(console.log); // => 3 VALUE = 'Event #1', then 'Event #2', then 'Event #3'
of(...events).subscribe(console.log); // => 3 VALUE = 'Event #1', then 'Event #2', then 'Event #3'
```
fromEvent
````typescript
const stream = fromEvent(this.myDOMElement.nativeElement, 'click')
  .subscribe(console.log)
;
````

## Resources
- https://youtu.be/-_XO5u-2UbQ?list=PLrbLGOB571zc3s8Mu4d1H-1PNzn6vc1Mb

### Playgrounds

- [Rxjs Playground](https://rxjs-playground.github.io/#/)
- [RxJS playground and examples](https://thinkrx.io/rxjs/)
- [Operators - Learn RxJS](https://www.learnrxjs.io/learn-rxjs/operators)
- [RxViz - Animated playground for Rx Observables](https://rxviz.com/)

