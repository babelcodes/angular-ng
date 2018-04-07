[Go back to home](https://github.com/babelcodes/angular-ng/tree/master/doc)

# Firebase

- https://firebase.google.com
  - > Console
  - > Create New Project

```
npm install --save angularfire2 firebase
```

- `firebase` is the low level API
- But we will use the highest level API called `angularfire2` through `AngularFireModule`

## Connection

- Create `FIREBASE_CONFIG`
- Inject it from `app.module` with `AngularFireModule`
- Update Firebase rules (in web console) to be able to READ/WRITE entities

## READ

- Create a Service who reads observables (`xxx$`) from `angularfire` API
- Provide this Service to the Component from the Module
- Inject this Service in the Component constructor
- Display items in the HTML view
