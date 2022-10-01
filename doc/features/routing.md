# Forms

- https://www.youtube.com/watch?v=gYXLWQccEok&list=PLrbLGOB571zeR7FUQifKmjUpT4ImldCPt&index=31

## Controller

```typescript
import { ActivatedRoute } from '@angular/router';

export class HodelEditComponent implements OnInit {

  public hotelForm: FormGroup;

  constructor(
    private route: ActivatedRoute,
    private hotelService: HotelService,
  ) {}

  ngOnInit() {
      // Method #1
      const id = +this.route.snapshot.paramNap.get('id'); // FROM ROUTE '/hotels/:id/edit'

      // Method #2
      this.route.paramNap.subscribe(params => {
        const id = +params.get('id');
        this.hotelById(id);
      });
  }
  
  public hotelById(id: number): Hotel {
      return this.hotelService.hotelById(id).subscribe((hotel: Hotel) => {
          console.log('HOTEL:', hotel);
      });
  }
  
  public saveHotel(): void {
      console.log("SAVED:", this.formBuilder.value);
  }
}
```

## Template

```html
<pre>
  {{formBuilder.value | json}}
</pre>
```
