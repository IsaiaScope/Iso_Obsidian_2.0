- https://www.30secondsofcode.org/articles/s/big-o-cheatsheet 

- To analyze the performance of an algorithm, we use Big O Notation
-   Big O Notation can give us a high level understanding of the time or space complexity of an algorithm
-   Big O Notation doesn't care about precision, only about general trends (linear? quadratic? constant?)
-   The time or space complexity (as measured by Big O) depends only on the algorithm, not the hardware used to run the algorithm

We say that an algorithm is *O(f(n))* if the number of simple operations the computer has to do is eventually less than a constant times *f(n)*, as *n* increases
-   f(n) could be linear (f(n) = n)
-   f(n) could be quadratic (f(n) = n  )
-   f(n) could be constant (f(n) = 1)
-   f(n) could be something entirely different!

*Big O Shorthands*
-   Arithmetic operations are constant
-   Variable assignment is constant
-   Accessing elements in an array (by index) or object (by key) is constant
-   In a loop, the the complexity is the length of the loop times the complexity of whatever happens inside of the loop
---
So far, we've been focusing on *time complexity*: how can we analyze the _runtime_ of an algorithm as the size of the inputs increases?
We can also use big O notation to analyze *space complexity*: how much additional memory do we need to allocate in order to run the code in our algorithm?
-   Most primitives (booleans, numbers, undefined, null) are constant space
-   Strings require O(_n_) space (where _n_ is the string length)
-   Reference types are generally O( _n_), where n is the length (for arrays) or the number of keys (for objects)

*Big O of Objects*
- Insertion -   *O(1)*
- Removal -   *O(1)*
- Searching -   *O(N)*
- Access -   *O(1)*

*Big O of Object Methods*
- Object.keys -   **O(N)**
- Object.values -   **O(N)**
- Object.entries -   **O(N)**
- hasOwnProperty -   **O(1)**

*Big O of Arrays*
- Insertion -   **It depends....**
- Removal -   **It depends....**
- Searching -   **O(N)**
- Access -   **O(1)**


*Big O of Array Operations*
-   push -   **O(1)**
-   pop -   **O(1)**
-   shift -   **O(N)**
-   unshift -   **O(N)**
-   concat -   **O(N)**
-   slice -   **O(N)**
-   splice -   **O(N)**
-   sort -   **O(N * log N)**
-   forEach/map/filter/reduce/etc. -   **O(N)**

`object is faster and need no order, array are great if need order but slower, when logic involved specific element like first or last is preferred to use method like pop, instead looping thought all array`