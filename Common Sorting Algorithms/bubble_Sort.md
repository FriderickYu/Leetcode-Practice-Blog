# Bubble Sort

## Implementation and Idea

<img src="../picture/Common%20Sorting%20Algorithms/bubble_sort/bubble_sort.jpg" width = "400" height = "370" alt="bubble sort" align=center/>

The above picture mainly shows the process of bubble sort.
Like bubble, the largest bubble will eventually flow to the top, and so on.
There are nine numbers, to move the largest number to the end, it needs 8 steps, so it's `nums.length - 1`.
To move the second largest number to the target position, it needs 7 steps, so it's `nums.length - 2`

* A loop that controls the entire process, basically it decides which position we want to put the new number
  * A nested loop that controls the detailed process
    * `nums[i] > nums[j] ? swap : none`
* return the original array

```java
public static int[] bubble_sort(int[] nums){
    for(int i = nums.length - 1; i >= 0; i --){
        for(int j = 0; j < i; j ++){
            if(nums[j] > nums[j + 1]){
                test.swap(nums, i, j);
            }
        }
    }
    return nums;
}
```

## Analysis

Time Complexity:

* Average: $O(n^2)$
* Worst: Consider that you want a ascending order, but the original array is completely descending, so you need to swap every elements, So $O(n^2)$
* Best: Consider that you want a ascending order, but it is already an ascending array, no need to really swap every element, just iterate the whole array, so $O(n)$

Space Complexity: Never use extra data structure, so $O(1)$

Stable: Bubble sort only swap adjacent elements, and does not swap two equal elements, therefore the relative positions of two same elements will not be changed, **Stable**

## Optimization

### Early end

If in a certain time, there is no swap happened, which means all elements are sorted. We can exit at this time. Setting a `boolean`to record whether there was swap or not

```java
public static int[] bubble_sort(int[] nums){
    for(int i = nums.length - 1; i >= 0; i --){
        // set a boolean to determine whether there is swap or not
        // if yes, return true, not return false;
        boolean swapped = false;
        for(int j = 0; j < i; j ++){
            if(nums[j] < nums[j + 1]){
                test.swap(nums, j, j + 1);
                swapped = true;
            }
        }
        // if no swap is happened, then exit the loop
        if(!swapped){
            break;
        }
    }
    return nums;
}
```
