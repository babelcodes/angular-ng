# Guards

## CanActivate

- https://www.youtube.com/watch?v=gAxYBtVJPl8&list=PLrbLGOB571zeR7FUQifKmjUpT4ImldCPt&index=30

## CanActivate

> Used to confirm exit from dirty form.

- See [Forms](form.md)

Create guard:
```shell
cd app/hotels/shared/guards

ng g g hotel-edit
>( ) CanActivate
 ( ) CanActivateChild
 ( ) CanDeactivate
 ( ) CanLoad
```

Write code of guard:
```typescript
// hotel-edit.guard.ts
import { ActivatedRoute } from '@angular/router';

@Injectable
export class HotelEditGuard implements CanDeactivate<HotelEditComponent> {

  canDeactivate(
    component: HotelEditComponent,
/*    
    currentRoute: ActivatedRouteSnapshot,
    currentState: RouterStateSnapshot,
    nextState?: RouterStateSnapshot,
*/): boolean /*Observable<boolean | UrlTree> | Promise<boolean>*/ {
    if (component.hotelForm.dirty) {
      const hotelName = component.hotelForm.get('hotelName').value;
      return confirm(`Voulez-vous annuler les changements sur ${hotelName}`);
    }
    return true;
  }

}
```

Register guard:
```typescript
// hotel-routing.module.ts
@NgModule({
  declarations: [
      HotelListComponent,
      HotelDetailComponent,
      StartRatingComponent,
  ],
  imports: [
      CommonModule,
      FormModule,
      RouterModule.forChild([
        { path: 'hotels/:id', component: 'HotelDetailCompoenent', canActivate: [HotelDetailGuard] },
        { path: 'hotels/:id/edit', component: 'HotelEditCompoenent', canDeactivate: [HotelEditGuard] }, // REGISTER HERE!
        { path: 'hotels', component: 'HotelListCompoenent' }
      ])
  ]
})
export class HotelModule {}
```
