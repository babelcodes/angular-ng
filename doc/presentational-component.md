[Go back to home](https://github.com/babelcodes/angular-ng/tree/master/doc)

# Dumb / Presentational component

* Accept data via inputs
* Emit data changes via event outputs

## @Input()

Stateless component

```
// my-feature.component.ts

  ...
  template: `<my-dumb [items]="passengers"></my-dumb>`
```


```
// my-dumb.component.ts

import { Input } from '@angular/core';

...
export class MyDumb {
  @Input()
  items: Passenger[];
}
```


## with *ngFor:

```
// my-feature.component.ts

  ...
  template: `<my-dumb-detail *ngFor="let p of passengers" [detail]="p"></my-dumb-detail>`
```

```
// my-dumb-detail.component.ts

import { Input } from '@angular/core';

...
export class MyDumbDetail {
  @Input()
  detail: Passenger;
}
```

## @Ouput()

Creating your own custom object (aka `edit` or `remove`)

```
// my-feature.component.ts

  ...
  template: `<my-dumb-detail *ngFor="let p of passengers" [detail]="p" (remove)="handleRemove($event)"></my-dumb-detail>`
...
})
export class MyFeature {
  handleRemove(event :Passenger) {
    this.passengers = this.passengers.filter( (p :Passenger) => { return p.id !== event.id; } );
  }

  handleEdit(event :Passenger) {
    this.passengers = this.passengers.map( (p :Passenger) => { return p.id === event.id ? Object.assign({}, p, event) : p; } );
  }
}
```

... A (custom) event called 'remove'


```
// my-dumb-detail.component.ts

import { Input, Output, EventEmitter } from '@angular/core';

@Component({
...
  template: `
    <input type="text" [value]="detail.fullName" (input)="onNameChange(name.value)" #name><!-- event called 'input' -->
    <button (click)="onRemove()">Remove</button>
  `
...
})
export class MyDumbDetail {
  @Input()
  detail: Passenger;

  @Output()
  remove: EventEmitter<any> = new EventEmitter();

  onNameChange(n: string) {
    this.detail.fullname = n;
  }
  
  onRemove() {
    this.remove.emit(this.detail);
  }

}
```

## ngOnChanges lifecycle hook

Because arrays are passed by reference, to change the value in PARENT only when validated in CHILD.


```
// my-dumb-detail.component.ts

import { OnChanges } from '@angular/core';

@Component({
...
})
export class MyDumbDetail implements OnChanges {

  ngOnChange(changes) {
    // console.log(changes);
    if (changes.detail) {
      this.detail = Object.assign({}, changes.detail.currentValue);
    }
  }

}
```

Display:

```
{
  detail: SimpleChange {
    currentValue: Object,
    previousValue: Object
  }
}
```

## Folders tree

```
.
|____my-feature
| |____components
| | |____my-dumb-detail
| | | |____my-dumb-detail.component.scss
| | | |____my-dumb-detail.component.ts
| |____containers
| | |____my-dumb-detail
| | | |____my-feature.component.scss
| | | |____my-feature.component.ts
| |____models
| | |____passenger.ts
| |____my-feature.module.ts
```


## Summary

```
┌──────────────────────────────────────────────────────────────────────────────────────────────────────────┐
│                                         <<< PARENT >>>                                                   │
│                                                                                                          │
│                                                                                                          │
│@Component                                                                                                │
│                                                                                                          │
│<!-- template -->                                                                                ▲        │
│<child                                                                                           │        │
│  *ngFor="let p of passengers" [detail]="p"                                                      │        │
│  (remove)="handleRemove($event)"                                                                │   (4)  │
│>                 ▲                                                                              │        │
│</child>          │                                                                              │        │
│                  │             (remove)                                                (remove) │        │
│                  └────────────────────────────┐                                                 │        │
│handleEvent(event :Passenger) {                │                                                 │   (5)  │
│  // DO THE REMOVE                             │                                                 │        │
│}                                              │                                                 │        │
│                                               │                                                 │        │
│   ┌───────────────────────────────────────────────────────────────────────────────────────┐     │        │
│   │                                     <<< CHILD >>>                                     │     │        │
│   │                                                                                       │     │        │
│   │                                                                                       │     │        │
│   │@Component                                                                             │     │        │
│   │                                                                                       │     │        │
│   │import { Input, Output, EventEmitter } from '@angular/core';                           │     │        │
│   │                                                                                       │     │        │
│   │                                                                                       │     │        │
│   │<!-- template -->                                                                      │     │        │
│   │<input type="text" [value]="detail.name" (input)="onNameChange(name.value)" #name>     │     │        │
│   │<button (click)="onRemove()"></button>                                                 │     │   (1)  │
│   │                                                                                       │     │        │
│   │                                                                                       │     │        │
│   │export class MyDumbDetail implements OnChanges {                                       │     │        │
│   │  @Input()                                                                             │     │        │
│   │  detail: Passenger;                                                                   │     │        │
│   │                                                                                       │     │        │
│   │  @Output()                                                                            │     │   (2)  │
│   │  remove: EventEmitter<any> = new EventEmitter();                                      │     │        │
│   │                                                                                       │     │        │
│   │  onNameChange(n: string) {                                                            │     │        │
│   │    this.detail.name = n;                                                              │     │        │
│   │  }                                                                                    │     │        │
│   │                                                                                       │     │        │
│   │  onRemove() {                                                                         │     │        │
│   │    this.remove.emit(this.detail);                                                     │     │   (3)  │
│   │  }                                                                                    │     │        │
│   │                                                                                       │     │        │
│   │  // To change the value in PARENT only when validated in CHILD                        │              │
│   │  ngOnChange(changes) {                                                                │              │
│   │    if (changes.detail) {                                                              │              │
│   │      this.detail = Object.assign({}, changes.detail.currentValue);                    │              │
│   │    }                                                                                  │              │
│   │  }                                                                                    │              │
│   │                                                                                       │              │
│   │}                                                                                      │              │
│   └───────────────────────────────────────────────────────────────────────────────────────┘              │
│                                                                                                          │
└──────────────────────────────────────────────────────────────────────────────────────────────────────────┘
```

