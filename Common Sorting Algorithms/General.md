# General knowledge and concepts

## Categories

Ten classic sorted algorithms:

* Based on comparison:
  * [Bubble Sort](bubble_Sort.md)
  * [Selection Sort](selection_Sort.md)
  * [Insertion Sort](insertion_Sort.md)
* Based on Divide and Conquer:
  * [Shell Sort](shell_Sort.md)
  * Merge Sort
  * Quick Sort
* Based on Data Structure:
  * Heap Sort
* Based on Count:
  * Counting Sort
  * Bucket Sort
  * Radix Sort

## Time Complexity and Space Complexity and Stability


| Sorted Algorithm | Average Time Complexity | Best Situation  | Worst Situation | Space Complexity | Stability |
| ------------------ | ------------------------- | ----------------- | ----------------- | ------------------ | ----------- |
| Bubble           | $O(n^2)$                | $O(n)$          | $O(n^2)$        | $O(1)$           | Stable    |
| Selection        | $O(n^2)$                | $O(n^2)$        | $O(n^2)$        | $O(1)$           | Unstable  |
| Insertion        | $O(n^2)$                | $O(n)$          | $O(n^2)$        | $O(1)$           | Stable    |
| Shell            | $O(nlogn)$              | $O(nlog^2n)$    | $O(nlog^2n)$    | $O(1)$           | Unstable  |
| Merge            | $O(nlogn)$              | $O(nlogn)$      | $O(nlogn)$      | $O(n)$           | Stable    |
| Quick            | $O(nlogn)$              | $O(nlogn)$      | $O(n^2)$        | $O(logn)$        | Unstable  |
| Heap             | $O(nlogn)$              | $O(nlogn)$      | $O(nlogn)$      | $O(1)$           | Unstable  |
| Counting         | $O(n + k)$              | $O(n + k)$      | $O(n + k)$      | $O(k)$           | Stable    |
| Bucket           | $O(n + k)$              | $O(n + k)$      | $O(n^2)$        | $O(n + k)$       | Stable    |
| Radix            | $O(n \times k)$         | $O(n \times k)$ | $O(n \times k)$ | $O(n + k)$       | Stable    |

### What is Stable

Stable means, after the sort, the relative position of the original same elements remain unchanged, compare to the original sequence

Like $nums={3, 1, 2_1, 2_2}$, 2 appears for two times. After the sort, if $nums={1, 2_2, 2_1, 3}$. The relative positions of `2` have been alerted, so it is unstable. Only if $nums={1, 2_1, 2_2, 3}$, then it is stable

## Swap

So, in `bubble sort`, `selection sort`, `insertion sort`, these kinds sorted algorithm need **compare** and **swap** elements, there are usually three ways to complete `sway`

1. use `temp` to swap `arr[i]` and `arr[j]`
2. use addition and substraction of `arr[i]` and `arr[j]`, but may cause **overflow**
3. bitwise operation, like exclusive OR operation(XOR)

### Use temp

```java
private void swap(int[] arr, int i, int j) {
    int tmp = arr[i];
    arr[i] = arr[j];
    arr[j] = tmp;
}
```

### Addition and Substraction

```java
private void swapCal(int[] arr, int i, int j) {
    if(i == j) return;
    arr[i] = arr[i] + arr[j]; // a = a + b
    arr[j] = arr[i] - arr[j]; // b = a - b
    arr[i] = arr[i] - arr[j]; // a = a - b
}
```

### [XOR](https://www.pcmag.com/encyclopedia/term/xor)

```java
private void swapXOR(int[] arr, int i, int j) {
    if(i == j) return;
    arr[i] = arr[i] ^ arr[j]; 
    arr[j] = arr[i] ^ arr[j]; 
    arr[i] = arr[i] ^ arr[j]; 
}
```
