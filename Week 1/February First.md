# Day 1 February 1st
## Leetcode 704 Binary Search
[Video Analysis](hhttps://www.bilibili.com/video/BV1fA4y1o715/?vd_source=e8f9779956746463d5471d7c18ccae92)

Including:
- Array
- Recursion
- Double pointers


**Notes**: 
- The list should be <font color='red'>sequential</font>.
- Adding two integer numbers may have some problems, because if numbers are long enough,
then <font color='red'> integer overflow </font> may occur.  
- A question should be known, is about <font color='red'> interval</font>, left and right. 



### Left-bounded and right-bounded
`[left, middle, right]`

`left <= right`
This means all elements are included in the search range. Therefore the right pointer
should be `array.length - 1`

Because the search range includes both left and right, so after the comparison

```java
if(array[middle] < target)
{
    left = middle + 1;
}
```

```java
if(array[middle] > target)
{
    right = middle - 1;
}
```
This is because the right element is already searched`(array[middle] > target)`, no need to 
search it again. 

#### Implementation
```java
class Solution {
    public int search(int[] nums, int target) {
        Arrays.sort(nums);
        // double pointers problem
        int left = 0;
        int right = nums.length - 1;
        // find the middle number, avoid integer overflow
        int middle = left + (right - left) / 2;
        // left-bounded and right-bounded
        while(left <= right){
            if(nums[middle] < target){
                left = middle + 1;
                middle = left + (right - left) / 2;
            }
            else if(nums[middle] > target){
                right = middle - 1;
                middle = left + (right - left) / 2;
            }
            else{
                return middle;
            }
        }
        return -1;
    }
}
```

### Left-bounded and right-unbounded
`[left, middle, right)`

`left < right`

As the above, the right element is not included, therefore the right pointer should be 
`right = array.length`

_The left pointer remains the same way as the left-bounded and right-bounded situation_
```java
if(array[middle] < target)
{
    left = middle + 1;
}
```

But, in this circumstance, it is quite different.

```java
if(array[middle] > target)
{
    right = middle;
}
```

Since the right element is not included, therefore after the comparison, right element
should be involved.

#### Implementation
```java
class Solution {
    public int search(int[] nums, int target) {
        Arrays.sort(nums);
        // double pointers problem
        int left = 0;
        int right = nums.length;
        // left-bounded and right-bounded
        while(left < right){
            // find the middle number, avoid integer overflow
            int middle = left + (right - left) / 2;
            if(nums[middle] < target){
                left = middle + 1;
            }
            else if(nums[middle] > target){
                right = middle;
            }
            else{
                return middle;
            }
        }
        return -1;
    }
}
```

### Recursion
#### Implementation
```java
class Solution {
    public int search(int[] nums, int target) {
        Arrays.sort(nums);
        int left = 0;
        int right = nums.length - 1;
        return Solution.recursion(nums, target, left, right);
        
    }
    // recursion
    public static int recursion(int[] nums, int target, int left, int right){
        while(left <= right){
            int middle = left + (right - left) / 2;
            if(nums[middle] > target){
                return recursion(nums, target, left, middle - 1);
            }
            else if(nums[middle] < target){
                return recursion(nums, target, middle + 1, right);
            }
            else{
                return middle;
            }
        }
        return -1;
    }
}
```
