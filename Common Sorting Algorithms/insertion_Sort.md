# Insertion Sort

## Implementation and Idea

Insertion sort, basically, is like shuffle the card. To line up in your hands ascendingly, the best way is to insert one card into an ordered list. Therefore, cards can be divided into two kinds, ordered and disordered, ordered list expands, and disordered list shrinks. Eventually, all cards will be ordered

* pointer `i` indicates the next element of the ordered list, the first element of the disordered list
  * pointer `j` starts from `i`, means this is the card we want to shuffle from `0~j`
  * compare `i` and `j`. if `arr[i] > arr[j]`, then swap from end to start(0~j)

<img src="../picture/Common%20Sorting%20Algorithms/insertion_sort/insertion_sort.jpg" width = "400" height = "220" alt="insertion sort" align=center/>

```java
public static int[] insertion_sort(int[] nums){
    for(int i = 1; i < nums.length; i ++){
        for(int j = i; j > 0; j --){
            if(nums[j] < nums[j - 1]){
                test.swap(nums, j, j - 1);
            }
            else{
                break;
            }
        }
    }
    return nums;
}
```

## Analysis

Time Complexity:

* Average: First loop is about `n-1` so it's $\frac{ n\times (n-1)}{2}$, $O(n^2)$
* Worst: You want an ascending order, but it is descending, so you need so swap every elements, $O(n^2)$
* Best: Already ordered, only thing needs to do is iterating all elements, $O(n)$

Space Complexity: $O(1)$

Stable: $[2, 3, 1_1, 1_2, 5]$, We want to let $1_2$ to insert into ordered list, when meeting with $1_1$, $1_2$ is still behind, so **stable**

## Optimization

### Binary Search

For instance, an array like `[1, 2, 3, 5, 4, 6]` only 4 is the element we should deal with. But, the list before 4 is already ordered. So this is a binary search problem

```java
public static void insertion_sort_binary_search(int[] nums){
    for(int i = 1; i < nums.length; i ++){
        int target = nums[i];
        int insertion_position = test.binary_search(nums, 0, i - 1, target);
        // move elements from insertion_position to i
        // back one step uniformly
        for(int j = i; j > insertion_position; j --){
            nums[j] = nums[j - 1];
        }
        nums[insertion_position] = target;
    }
}
public static int binary_search(int[] nums, int low, int right, int target){
    while(low <= right){
        int middle = low + (right - low) / 2;
        if(nums[middle] > target){
            right = middle - 1;
        }
        else if(nums[middle] < target){
            low = middle + 1;
        }
        else{
            return middle;
        }
    }
    return low;
}
```

Although the search changes to $O(nlogn)$, but moving the rest of elements still needs $O(n^2)$
