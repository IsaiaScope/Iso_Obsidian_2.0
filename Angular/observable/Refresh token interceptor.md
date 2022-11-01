- [[interceptor-refreshtoken.svg]]
- quando facciamo una chiamata ci sottoscriviamo a allo stream del token (***token$***)preso dallo store (che contiene il valore attuale del token che può essere una stringa o null), fin quando il token è null non facciamo nessuna chiamata, ma quando il token ha un valore proseguiamo nello stream;
``` typescript

  token$ = this.store.select(selectAuthState).pipe(
    map(({ accessToken }) => accessToken),
    filter((accessToken) => accessToken !== null)
  )
  
```
- settiamo i nostri eventuali header con un ***map*** , prendiamo solo il primo valore emesso (cioè il valore dello stream quando ci agganciamo) e facciamo la chiamata con un ***exhaustMap*** che in pratica se arrivano altri valori sullo stream vengono ignorati. Questo permette di generare un nuovo stream da quello precedente che va a chiudersi con la request effettiva.
``` typescript

return this.token$.pipe(
      map((accessToken) => this.addHeaders(req, accessToken)),
      take(1),
      exhaustMap((req) => {
        // console.log(`old stream`);
        return next.handle(req);
      }), 
      
```
- a ciò agg. ***catchError*** che permette di catturare eventuali errori e noi vogliamo prendere le chiamate che vanno in 403 (ERROR.forbidden) che significa che l'accessToken è scaduto (logica server side che restituisce 403 all'expire del token) .
- se ciò avviene ci sottoscriviamo a ***isRefreshing$*** che contiene il valore isRefreshing dello store; il valore di tale observable è true se sta richiamando l'API di refresh al contrario è false; a questo stream prendiamo solo il valore attuale con ***take(1)***.
``` typescript

catchError((err: HttpErrorResponse | ErrorEvent) => {
        if (err instanceof HttpErrorResponse && err.status === ERROR.forbidden)
          return this.isRefreshing$.pipe(
            tap((isRefreshing) => {
              !isRefreshing && this.store.dispatch(refreshToken());
            }),
            switchMap(() => {
              return this.token$.pipe(
                switchMap((accessToken) => {
                  // console.log(`last call`);
                  return next.handle(this.addHeaders(req, accessToken));
                })
              );
            })
          );
        return this.errSrv.handleErr(err, ERROR_TYPES.generalApi);
      })
      
```
- ***se isRefreshing$ è false richiamiamo il refresh del token, quindi setto il token a null sfruttando lo store e ci spostiamo di nuovo in ascolto sullo stream del token; quando arriva il valore del token facciamo una nuova chiamata (in questo punto avviene la magia perchè token$ filtra le chiamate in cui il token è null; quindi tutte le chiamate che entrano è come se restassero in ascolto pendenti in attesa di un nuovo valore del token perchè quando una chiamata entra nell'interceptor crea tutta questa nuova stream ma non riesce a chiuderla e si blocca li. Quando poi arriva il valore tute le stream aperte effettuano le proprioe chiamate e concludono lo stream).***
``` typescript

@Injectable()
export class ApiInterceptorService implements HttpInterceptor {
  token$ = this.store.select(selectAuthState).pipe(
    map(({ accessToken }) => accessToken),
    filter((accessToken) => accessToken !== null),
    take(1)
  );

  isRefreshing$ = this.store.select(selectAuthState).pipe(
    map(({ isRefreshing }) => isRefreshing),
    take(1)
  );
  
  constructor(private store: Store, private errSrv: ErrorService) {}

  intercept(
    req: HttpRequest<any>,
    next: HttpHandler
  ): Observable<HttpEvent<any>> {
  // handling api with endpoit that includes assets/svgs/icons
    if (req.url.indexOf('assets/svgs/icons') > -1) {
      return next.handle(req);
    }
    
    // handling api with endpoit /login, /refres, /loguot
    if (
      req.url.indexOf(ROUTES.endpoints.login) > -1 ||
      req.url.indexOf(ROUTES.endpoints.refresh) > -1 ||
      req.url.indexOf(ROUTES.endpoints.logout) > -1
    ) {
      return next.handle(req).pipe(
        catchError((err: HttpErrorResponse | ErrorEvent) => {
          return this.errSrv.handleErr(err, ERROR_TYPES.authApi);
        })
      );
    }
    
    // handling api with different endpoit from /login, /refres, /loguot
    return this.token$.pipe(
      exhaustMap((req) => {
       return next.handle(this.addHeaders(req, accessToken));
      }),
      catchError((err: HttpErrorResponse | ErrorEvent) => {
        if (err instanceof HttpErrorResponse && err.status === ERROR.forbidden)
          return this.isRefreshing$.pipe(
            tap((isRefreshing) => {
              !isRefreshing && this.store.dispatch(refreshToken());
            }),
            switchMap(() => {
              return this.token$.pipe(
                switchMap((accessToken) => {
                  // console.log(`last call`);
                  return next.handle(this.addHeaders(req, accessToken));
                })
              );
            })
          );
        return this.errSrv.handleErr(err, ERROR_TYPES.generalApi);
      })
    );
  }

  
	// set headers
  addHeaders(req: HttpRequest<any>, accessToken: string | null) {
    const _headers: {
      [string: string]: string;
    } = {};
    accessToken && (_headers['Authorization'] = `Bearer ${accessToken}`);
    return req.clone({
      setHeaders: _headers,
      withCredentials: true,
    });
  }
}
      
```