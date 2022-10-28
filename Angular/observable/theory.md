#### Install ngRx
```console
ng add @ngrx/store@latest
```
-  remember to add to app.module imports section after created a store
```console
StoreModule.forRoot(reducers, {metaReducers}),
```
---
#### Add library for devTool ngRx
1. remember to add to app.module imports section
```console
ng add @ngrx/store-devtools@latest
```
2. remember to add to app.module imports
```typescript
StoreDevtoolsModule.instrument({ maxAge: 25, logOnly: env.production }),
```
---
#### Add library for effects ngRx
- remember to register your effects to store module or app module
- ex. EffectsModule.forRoot([AuthEffects]),
```console
ng add @ngrx/effects@latest
```
---
#### Store Logic
1. dispatch action from any store prop and pass the prop for that action
```typescript
this.store.dispatch(actionFromAnyStoreProp({ data }));
```
2. get store value, is a subscription
```typescript
$streamName: Observable<any> = this.store.select<any>(neededSate);
```
---