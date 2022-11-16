#### flat()
- The flat() method creates a new array with all sub-array elements concatenated into it recursively up to the specified depth. Also remove empty slot in arrays
- [Array.prototype.flat()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/flat)
1. transform string[ ][ ] => string[ ] 
	- putting = [ ] we cover the situation of defArr or classesArr could be undefined 
```ts
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