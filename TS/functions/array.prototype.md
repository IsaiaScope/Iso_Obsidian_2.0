#### flat()
- 
```typescript
  setClasses(
    defArr: string[],
    keepDef: boolean = true,
    classesArr: string[],
    ...args: Array<string[]>
  ) {
    const moreStyle = [...args.flat()];
    if (keepDef) {
      return [...defArr, ...classesArr, ...moreStyle];
    } else {
      return [...classesArr, ...moreStyle];
    }
  }
```