[Go back to home](https://github.com/babelcodes/angular-ng/tree/master/doc)

# RxJS & Observables

> Reactive Extensions Library for JavaScript

- https://rxjs.dev
- [Tutoriel Angular #25 - RxJS et les Observables + exemples dynamiques](https://www.youtube.com/watch?v=aZcPIkSKHv4&list=PLrbLGOB571zeR7FUQifKmjUpT4ImldCPt&index=26&t=5s)
- [Tutoriel Angular #26 - HttpClient & Error Handling avec RxJS](https://www.youtube.com/watch?v=w1PLZ6WptW4&list=PLrbLGOB571zeR7FUQifKmjUpT4ImldCPt&index=27&t=63s)
- 

## Reference

### Observable

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

