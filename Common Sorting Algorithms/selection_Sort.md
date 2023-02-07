# Selection Sort

## Implementation and Idea

The basic idea is, set `minIndex` to record the index of the minimum element. First assuming that the first element is the minimum element, and `minIndex = 0`. Then comparing `arr[minIndex]` to the rest of elements. When some one is smaller, let `minIndex` equal to this number's index. In the end of this round, swap the first element and the smallest element, `arr[minIndex]`

* A big loop, from 0 to `arr.length - 2`, because if `n - 1` is sorted, then no need to deal with the last number
  * set `minIndex` from 0 to `arr.length - 2`
  * A nest loop, from 1 to arr.length - 1
    * if `arr[j] < arr[minIndex]`, then `minIndex = j`
  * swap the minimum value's index
* return to array

<img src="../picture/Common%20Sorting%20Algorithms/selection_sort/selection%20sort.jpg" width = "400" height = "250" alt="selection sort" align=center/>

```java
public static int[] selection_sort(int[] nums){
    for(int i = 0; i < nums.length - 1; i ++){
        int minIndex = i;
        for(int j = i + 1; j < nums.length; j ++){
            if(nums[minIndex] > nums[j]){
                minIndex = j;
            }
        }
        test.swap(nums, i, minIndex);
    }
    return nums;
}
```

## Analysis

Time Complexity: The first round needs `n - 1` times, the last needs 1 time, overall needs $\frac {n \times (n-1)}{2}$, so $O(n^2)$

Space Complexity: $O(1)$

Stable: It is not just about adjacent swap, like to find the minimum in a certain round, will eventually swap with the position which is initialized in this round. If there is an element equal to this initialized element, it will break the stability, so **unstable**

**Hint**: think about `[7, 7, 2]`, in the first round, 2 will swap with the first 7, then 7 and 7 will disturb the stability

## Optimization

So in the basic version, it only seek the **minimum** value. How about to seek **maximum** value simultaneously?

This optimization will reduce the iteration time by half. However, it should be noticed that operations in each round is increased. Therefore, this could only improve a little. **Coefficient rather than exponent**

```java
public static int[] selection_sort(int[] nums){
    /* In each round, two numbers will be settled down, minimum and maximum.
     * For instance, first round, 0 and nums.length -1;
     * second round, 1 and nums.length - 1 - 1
     * third round, 2 and nums.length - 1 - 2
     * */
    for(int i = 0; i < nums.length - 1 - i; i ++){
        int minIndex = i;
        int maxIndex = i;
        for(int j = i + 1; j < nums.length - i; j ++){
            if(nums[minIndex] > nums[j]){
                minIndex = j;
            }
            if(nums[maxIndex] < nums[j]){
                maxIndex = j;
            }
        }
        /* if minIndex is equal to maxIndex, means all unsorted elements are equal
        * No need to continue
        * */
        if(minIndex == maxIndex){
            break;
        }
        test.swap(nums, i, minIndex);
        /* When swap i and minIndex, it is very possible that i is maxIndex
        * Like [1, 0, 0, 1, 0, 1]
        * In this array, minIndex and maxIndex are 0 initially,
        * but 1 is the max value of this array
        * */
        if(maxIndex == i){
            maxIndex = minIndex;
        }
        test.swap(nums, nums.length - 1 - i, maxIndex);
    }
    return nums;
}
```
