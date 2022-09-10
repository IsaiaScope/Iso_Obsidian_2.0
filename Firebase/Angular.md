


#### generation
- remember to add to app.module imports section
```console
ng add @ngrx/store-devtools@latest
```
-  remember to add to app.module imports
```typescript
StoreDevtoolsModule.instrument({ maxAge: 25, logOnly: env.production }),
```
---