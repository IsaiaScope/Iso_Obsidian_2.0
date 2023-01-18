#### Telling JavaScript how to sort
Examples: native method but this approach is bad
 - [Array.prototype.sort()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/sort)
```js
function numberCompare(num1, num2) {
  return num1 - num2;
}

[ 6, 4, 15, 10 ].sort(numberCompare);
// [ 4, 6, 10, 15 ]
```

```js
function compareByLen(str1, str2) {
  return str1.length - str2.length;
}

[ "Steele", "Colt", "Data Structures", "Algorithms" ].sort(compareByLen);
// [ "Colt", "Steele", "Algorithms", "Data Structures" ]
```
---

| Algorithm | Time Complexity (Best) | Time Complexity (Average) | Time Complexity (Worst) | Space Complexity |
| --- | --- | --- | --- | --- |
| Bubble Sort | O(_n_) | O(_n_) | O(_n_) | O(1) |
| Insertion Sort | O(_n_) | O(_n_) | O(_n_) | O(1) |
| Selection Sort | O(_n_) | O(_n_) | O(_n_) | O(1) |

-   Sorting is fundamental!
-   Bubble sort, selection sort, and insertion sort are all roughly equivalent
-   All have average time complexities that are quadratic
-   We can do better...but we need more complex algorithms!

*other more complex sort but faster than previous ones:*
- Merge Sort 
- Quinck Sort 
- Radix Sort 

