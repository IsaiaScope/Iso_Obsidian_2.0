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
2. simple example usage
```ts
const arr1 = [1, 2, [3, 4]];
arr1.flat();
// [1, 2, 3, 4]

const arr2 = [1, 2, [3, 4, [5, 6]]];
arr2.flat();
// [1, 2, 3, 4, [5, 6]]

const arr3 = [1, 2, [3, 4, [5, 6]]];
arr3.flat(2);
// [1, 2, 3, 4, 5, 6]

const arr4 = [1, 2, [3, 4, [5, 6, [7, 8, [9, 10]]]]];
arr4.flat(Infinity);
// [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
```
---
#### flatmap()
- The flatMap() method returns a new array formed by applying a given callback function to each element of the array, and then flattening the result by one level. It is identical to a map() followed by a flat() of depth 1 (`arr.map(...args).flat()`), but slightly more efficient than calling those two methods separately.
- [Array.prototype.flatMap()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/flatMap)
```ts
const arr1 = [1, 2, 3, 4];

arr1.flatMap((x) => [x * 2]);
// [2, 4, 6, 8]

// only one level is flattened
arr1.flatMap((x) => [[x * 2]]);
// [[2], [4], [6], [8]]
```
---