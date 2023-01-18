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

#### Other more complex sort but faster than previous ones:
- *Merge Sort *

| Time Complexity (Best) | Time Complexity (Average) | Time Complexity (Worst) | Space Complexity |
| --- | --- | --- | --- |
| O(_n_ log _n_) | O(_n_ log _n_) | O(_n_ log _n_) | O(_n_) |
---
- *Quinck Sort *

| Time Complexity (Best) | Time Complexity (Average) | Time Complexity (Worst) | Space Complexity |
| --- | --- | --- | --- |
| O(_n_ log _n_) | O(_n_ log _n_) | O(_n_  ) | O(log _n_) |
---
- *Radix Sort *

| Time Complexity (Best) | Time Complexity (Average) | Time Complexity (Worst) | Space Complexity |
| --- | --- | --- | --- |
| O(_nk_) | O(_nk_) | O(_nk_) | O(_n + k_) |
-   _n_ - length of array
-   _k_ - number of digits(average)
---
*Recap*
-   Merge sort and quick sort are standard efficient sorting algorithms
-   Quick sort can be slow in the worst case, but is comparable to merge sort on average
-   Merge sort takes up more memory because it creates a new array (in-place merge sorts exist, but they are really complex!)
-   Radix sort is a fast sorting algorithm for numbers
-   Radix sort exploits place value to sort numbers in linear time (for a fixed number of digits)
