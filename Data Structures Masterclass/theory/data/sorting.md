# Telling JavaScript how to sort

Examples

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

[ "Steele", "Colt", "Data Structures", "Algorithms" ]
  .sort(compareByLen);
// [ "Colt", "Steele", "Algorithms", "Data Structures" ]
```


---

| Algorithm | Time Complexity (Best) | Time Complexity (Average) | Time Complexity (Worst) | Space Complexity |
| --- | --- | --- | --- | --- |
| Bubble Sort | O(_n_) | O(_n_  ) | O(_n_  ) | O(1) |
| Insertion Sort | O(_n_) | O(_n_  ) | O(_n_  ) | O(1) |
| Selection Sort | O(_n_  ) | O(_n_  ) | O(_n_  ) | O(1) |

-   Sorting is _fun_damental!
-   Bubble sort, selection sort, and insertion sort are all roughly equivalent
-   All have average time complexities that are quadratic
-   We can do better...but we need more complex algorithms!