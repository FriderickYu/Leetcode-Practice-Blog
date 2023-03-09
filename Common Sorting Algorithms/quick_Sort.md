# Quick Sort

Quick sort is also a sort, uses divide and conquer. Two core factors, **determining main axis**, and **partition**

1. determining main axis, divide the entire array
2. putting elements smaller than main axis before the main axis, and putting elements bigger than main axis after the main axis
3. once dealing with main axis, iterating two parts, before the main axis and after the main axis

## Implementation Idea

Partition: once we determine the main axis, we put elements smaller than main axis, before the main axis, and put elements bigger than main axis, after the main axis. Finally, return to the index of the main axis

```java
public static int partition(int[] arr, int l, int r) {
// select the first element as the main axis
int j = l + 1;
for (int i = j; i <= r; i++) {
    // compare the main axis and others
    if (arr[i] < arr[l]) {
        swap(arr, i, j);
        j++;
    }
}
// finally, we rearrange the main axis
swap(arr, l, j - 1);
return j - 1;
```

```java
public int[] quickSortSimple(int[] arr) {
    if (arr.length < 2) return arr;
    quickSortSimple(arr, 0, arr.length - 1);
    return arr;
}
private void quickSortSimple(int[] arr, int l, int r) {
    if (l < r) {
        int p = partition(arr, l, r);
        quickSortSimple(arr, l, p - 1);
        quickSortSimple(arr, p + 1, r);
    }
}
```

## Analysis

Time Complexity: $O(nlogn)$

Space Complexity: $O(logn)$

Stable: once we determine the position of main axis, we will swap the element on the index of main axis, and the last element which is smaller than main axis, this will sabotage the stable. Ex: `7 2 4 4 8 9`, suppose 7 is the main axis, and we need to swap 7 and the second 4, become to `4 2 4 7 8 9`, then positions of two 5 are changed. So, **unstable**

## Optimization
