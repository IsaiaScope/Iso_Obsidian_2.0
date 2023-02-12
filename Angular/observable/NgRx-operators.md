#### exhaustMap()
- permette di creare una nuova stream con il valore attuale della stream a cui ti sottoscrivi e tutti i nuovi valori che arrivano sulla stream da cui abbiamo staccato il 'branch' vengano ignorati fino a quando non viene conclusa il primo flusso, in questo esempio permette di evitare che un utente possa 'spammare' sul tasto login perchè tutte le interazioni successive alla prima vengono ignorate fino a quando non si conclude lo stream. Questo caso lo stream si conclude quando riceviamo il token dal backend e di conseguenza viene dispatchata l'azione di loginSuccess
```typescript

  loginAction$ = createEffect(() =>
    this.actions$.pipe(
      ofType(authAction.login),
      exhaustMap((loginFormValue) =>
        this.authSrv.login(loginFormValue).pipe(
          tap(console.log),
          map((accessToken) => authAction.loginSuccess(accessToken))
        )
      )
    )
  );

```
---