#### Dispatch Action
```typescript
this.store.dispatch(actionFromAnyStoreProp({ data }));
```
---
#### Get Store Value
```typescript
$streamName: Observable<any> = this.store.select<any>(neededSate);
```
---