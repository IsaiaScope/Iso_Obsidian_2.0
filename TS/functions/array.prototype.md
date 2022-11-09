#### flat()
-  flat transforms an array of array in a single array of every element of each array
- ex: string[ ][ ] => string[ ] 
- putting = [ ] we cover the situation of defArr or classesArr could be undefined 
```typescript
  setClasses(
    defArr: string[] = [],
    keepDef: boolean = true,
    classesArr: string[] = [],
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