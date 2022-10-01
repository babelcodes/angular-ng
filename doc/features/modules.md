# Modules

- https://www.youtube.com/watch?v=gYXLWQccEok&list=PLrbLGOB571zeR7FUQifKmjUpT4ImldCPt&index=31
- https://www.youtube.com/watch?v=TYI6vWcYENE&list=PLrbLGOB571zeR7FUQifKmjUpT4ImldCPt&index=32
- https://www.youtube.com/watch?v=yQ5OJP5pwEM&list=PLrbLGOB571zeR7FUQifKmjUpT4ImldCPt&index=33
- https://www.youtube.com/watch?v=YWODt4wzziU&list=PLrbLGOB571zeR7FUQifKmjUpT4ImldCPt&index=34

```shell
app
  - hotels
    - hotels-detail
    - hotels-list
  - shared
    - guards
    - models
    - services
```

```shell
$ ng g m hotels/hotel --flat -m app
```

```typescript
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
        { path: 'hotels', component: 'HotelListCompoenent' }
      ])
  ]
})
export class HotelModule {}
```

Modules:
- App
- Hotel
- Shared
- Routing



```shell
$ ng g m hotels/hotel-routing --flat
```

```typescript
@NgModule({
  imports: [
      RouterModule.forChild([
        { path: 'hotels/:id', component: 'HotelDetailCompoenent', canActivate: [HotelDetailGuard] },
        { path: 'hotels', component: 'HotelListCompoenent' }
      ])
  ],
  exports: [RouterModule],
})
export class HotelRoutingModule {}
```




Architecture:
```
BrowserModule

RouterModule HttpClientModule

                            SharedModule CommonModule FormsModule RouterModule StartRoutingComponent

  AppModule<----------------HotelModule
  
        HomeComponent    HotelListComponent
  AppComponent          HotelDetailComponent

```
