#### ==Generics==
```typescript
function logAndEcho <T> ( val : T ) {
  console.log ( val ) ;
  return val ;
}
logAndEcho<string>( ' Hi there ! ' ) . split ( ' ' ) ;
```
---