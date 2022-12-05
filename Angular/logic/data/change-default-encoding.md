1.  create a new class to override HttpParameterCodec default; switching between encodeURI() (default one) to encodeURIComponent()
2. `NOTE` This was fixed in version 14 => TO CHECK
4. [stackoverflow](https://stackoverflow.com/questions/49438737/how-to-escape-angular-httpparams)
5. [encodeURI() VS encodeURIComponent()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/encodeURIComponent)
```ts
import { HttpParameterCodec } from '@angular/common/http';

export class CustomEncoderUri implements HttpParameterCodec {

    encodeKey(key: string): string {
        return encodeURIComponent(key);
    }
    encodeValue(value: string): string {
            return encodeURIComponent(value);
    }
    decodeKey(key: string): string {
            return decodeURIComponent(key);
    }
    decodeValue(value: string): string {
            return decodeURIComponent(value);
    }
}
```
---
3.  usage example
	- `NOTE` isn't a Singleton so u can create multiple instances
```ts
  getBoundaries(data) {
    const params = new HttpParams({
      encoder: new CustomEncoderUri(),
      fromObject: data,
    });
    return this.http
      .get(`${this.endpoint}/boundaries`, {
        params,
      })
      .pipe(map((next: ResponseModel) => next.response.hits));
  }
```

---
