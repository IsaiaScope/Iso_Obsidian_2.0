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
- remember to add to app.module imports section
```console
ng add @ngrx/store-devtools@latest
```
-  remember to add to app.module imports
```typescript
StoreDevtoolsModule.instrument({ maxAge: 25, logOnly: env.production }),
```
---
