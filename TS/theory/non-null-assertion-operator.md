- A new `!` post-fix expression operator may be used to assert that its operand is non-null and non-undefined in contexts where the type checker is unable to conclude that fact. Specifically, the operation `x!` produces a value of the type of `x` with `null` and `undefined` excluded. Similar to type assertions of the forms `<T>x` and `x as T`, the `!` non-null assertion operator is simply removed in the emitted JavaScript code.

1. ex
```typescript
function validateEntity(e?: Entity) {    
	// Throw exception if e is null or invalid entity 
}  

function processEntity(e?: Entity) {    
	validateEntity(e);    
	let s = e!.name; // Assert that e is non-null and access name 
} 
```
---
2. async pipe
	- u can avoid to add ngif before beacuse (isLoggingIn$ | async)***!*** is sayin that isLoggingIn$ is always different from undefined or null
```typescript
<app-form-btn
        [setUp]="{ label: 'login' }"
        [disable]="(isLoggingIn$ | async)!"
 ></app-form-btn>
```
---