```typescript
const removeKey = (invalidValue: any[]) => (obj: Record<string, string>) => {
  return Object.keys(obj).reduce((acc: Record<string, string>, key: string) => {
    !invalidValue.includes(obj[key]) && (acc[key] = obj[key]);
    return acc;
  }, {});
};
  
export const noEmptyStringKey = removeKey(['']);
export const noEmptyKeys = removeKey(['undefined', 'null']);
```

  



