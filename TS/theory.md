#### ==Generics==
```typescript
function logAndEcho <T> ( val : T ) {
  console.log ( val ) ;
  return val ;
}
logAndEcho<string>( ' Hi there ! ' ) . split ( ' ' ) ;
```
---
```typescript
const getJSON = <T>(config: { url: string }): Promise<T> => {

    const fetchConfig = ({ method: 'GET' });

    return fetch(config.url, fetchConfig).then<T>(response => response.json());

  }
```
---
