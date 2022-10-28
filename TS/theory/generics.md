1. Generics simple ex
```typescript
function logAndEcho <T> ( val : T ) {
  console.log ( val ) ;
  return val ;
}

logAndEcho<string>( ' Hi there ! ' ) . split ( ' ' ) 
```
---
2. Generics ==Promise==
```typescript
export function readeFileContent<T>(inputElm: HTMLInputElement): Promise<T> {

  const file = inputElm.files[0];

  if (!file) {

    return Promise.reject("File not found");

  }

  return new Promise<T>((res, rej) => {

    const reader = new FileReader();

    reader.readAsText(file, "UTF-8");

    // @ts-ignore

    reader.onload = (evt) => res(evt.target.result);

    reader.onerror = () => rej("error reading file");

  });

}
```
---
