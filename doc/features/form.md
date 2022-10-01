# Forms

- https://www.youtube.com/watch?v=gYXLWQccEok&list=PLrbLGOB571zeR7FUQifKmjUpT4ImldCPt&index=31

## Controller

```typescript
import { ActivatedRoute } from '@angular/router';

export class HodelEditComponent implements OnInit {

  public hotelForm: FormGroup;
  public hotel: Hotel;

  constructor(
    private route: ActivatedRoute, // SEE ./routing.md
    private hotelService: HotelService, // SEE ./routing.md
    private formBuilder: FormBuilder,
  ) {}

  ngOnInit() {
    this.hotelForm = this.formBuilder.group({
      hotelName: ['', Validators.required],
      hotelPrice: ['', Validators.required],
      starRating: [''],
      decsription: [''],
    });

    this.route.paramNap.subscribe(params => {
      const id = +params.get('id');
      this.loadHotelById(id);
    });
  }
  
  public loadHotelById(id: string): void {
    return this.hotelService.hotelById(id).subscribe((hotel: Hotel) => {
      this.hotel = hotel;
      
      this.hotelForm.patchValue({
          hotelName: this.hotel.name,
          hotelPrice: this.hotel.price,
          starRating: this.hotel.rating,
          description: this.hotel.description,
      });
    });
  }

  public saveHotel(): void {
      console.log("SAVED:", this.formBuilder.value);
  }

}
```

## Template

```html
<form novalidate [formGroup]="hotelForm" (ngSubmit)="saveHotel()">
  <input
    id="hotelNameId"
    type="text"
    class="form-control"
    formControlName="hotelName"
    placeholder="Hotel Name (obligatoire)"
  >
  
  <button type="submit">Save</button>
  <button type="button" routerLink="/hotels">Cancel</button>
</form>

<pre>
  {{formBuilder.value | json}}
</pre>
```
