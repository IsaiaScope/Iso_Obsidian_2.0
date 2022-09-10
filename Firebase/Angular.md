- https://github.com/angular/angularfire => official Angular and Firebase library
- https://github.com/angular/angularfire/blob/master/docs/install-and-setup.md
---
#### Install AngularFire and Firebase
- during installation u have to login in to authenticate for db work
---

```console
ng add @angular/fire
```

#### Auth
-  remember to add to app.module imports AngularFireAuthModule
```typescript
StoreDevtoolsModule.instrument({ maxAge: 25, logOnly: env.production }),
```
---