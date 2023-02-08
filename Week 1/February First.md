# Day 1 February 1st

## [Leetcode 704 Binary Search](https://leetcode.com/problems/binary-search/)

[Video Analysis](https://www.bilibili.com/video/BV1fA4y1o715/?vd_source=e8f9779956746463d5471d7c18ccae92)

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

## Similar Questions

### [Leetcode 35 Search Insert Position](https://leetcode.com/problems/search-insert-position/)

Including:

- Array
- Binary search
- Double pointers

#### Straight-forward

Just simply imitate binary search, use a variable to record <font color="red">middle </font>

##### Implementation

```java
public int searchInsert(int[] nums, int target) {
    int left = 0;
    int right = nums.length - 1;
    int marked_index = 0;
    while(left <= right){
        int middle = left + (right - left) / 2;
        marked_index = middle;
        if(nums[middle] < target){
            left = middle + 1;
        }
        else if(nums[middle] > target){
            right = middle - 1;
        }
        else{
            break;
        }
    }
    if(nums[marked_index] < target){
        return marked_index + 1;
    }
    else if(nums[marked_index] > target){
        if(marked_index == 0){
            return marked_index;
        }
        else{
            return marked_index;
        }
    }
    else{
        return marked_index;
    }
    }
```

#### Improvement

Actually, the left pointer can be used to mark which position we should put the value in,
Because, if left pass the right position, means:

- Target is larger than the `array[left]`, after `left + 1`, then the position left point,
  is exactly the correct position we want to put the target in
- Target is smaller than the `array[left]`, after `right + 1`, then those numbers who are
  larger than target should move on, and putting target in this position

```java
int left = 0, right = nums.length - 1;
while(left <= right) {
    int mid = (left + right) / 2;
    if(nums[mid] == target) {
        return mid;
    } else if(nums[mid] < target) {
        left = mid + 1;
    } else {
        right = mid - 1;
    }
}
return left;
```

### [Leetcode 34 Find First and Last Position of Element in Sorted Array](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/)

Including:

- Array
- Binary search
- Double pointers
- Built-in function

#### Stupid One

Using built-in function of `ArrayList`, `indexOf`and`lastIndexOf`, put every elements
in this `ArrayList`

##### Implementation

```java
ArrayList<Integer> array = new ArrayList<>();
for(int i = 0; i < nums.length; i ++){
    array.add(nums[i]);
}
return new int[]{array.indexOf(target), array.lastIndexOf(target)};
  
```

#### Binary search

Based on binary search, if this number is existed, then implement two-way search:

- Left Search: searching towards left, if the previous number is not equal to target,
  stop and record this position
- Right Search: searching towards right, if the next number is not equal to target,
  stop and record this position

```java
int result = Solution.binarySearch(nums, target);
int[] ans = {-1, -1};
if(result != -1){
    ans[0] = result;
    ans[1] = result;
    while(result - 1 >= 0 && nums[result - 1] == target){
        ans[0] = -- result;
    }
    while(result + 1 < nums.length && nums[result + 1] == target){
        ans[1] = ++ result;
    }
}
return ans;
```

## [Leetcode 27 Remove Element](https://leetcode.com/problems/remove-element/)

[Video Analysis](https://www.bilibili.com/video/BV12A4y1Z7LP)

Including:

- Array
- Double pointers
- Fast-slow pointers

**Notes**:

- Do not allocate extra space for another array
- Time complexity is `O(1)` strictly

`Array` is a tricky data structure. In `Java`, we can not just remove elements in it,
unless we can use other data structure like `ArrayList`, which contains functions like
`remove`, but this is not a qualified methods

This question can be solved by using double pointers, combining with fast-slow.
Faster pointer indicates which elements we want to reserve, slower pointer indicates
where we want to put elements into. Because slower pointer indicates the position, therefore,
at the end, `return slow` actually shows the size of `Array` which only keeps elements we want

### Implementation

```java
int slow = 0;
for(int fast = 0; fast < nums.length; fast ++){
    if(nums[fast] != val){
        nums[slow] = nums[fast];
        slow ++;
    }
}
return slow;
```

## [Leetcode 69 Sqrt(x)](https://leetcode.com/problems/sqrtx/)

Including:

* Binary Search

Because the integer part of sqrt root of x, **result**, always following the part $k^2 \leq x$

so the maximum value of k, is the **result** we want to find

```java
int low = 1;
int high = x;
int result = 0;
if(x == 0){
    return 0;
}
if(x == 1){
    return 1;
}
while(low <= high){
    int middle = low + (high - low) / 2;
    if((long)middle * middle <= x){
        result = middle;
        low = middle + 1;
    }
    else{
        high = middle - 1;
    }
}
return result;
```

## [Leetcode 2529 Maximum Count of Positive Integer and Negative Integer](https://leetcode.com/problems/maximum-count-of-positive-integer-and-negative-integer/description/)

Including:

* Array
* Binary Search

This is a **non-decreasing** order

`[-2, -1, -1, 1, 2, 3]`

The core of this problem is to find the boundary that could divide both positive and negative numbers

So, we want to find the **right boundary** of negative numbers

```java
if(nums[middle] < 0){
    low = middle + 1;
}
else{
    high = middle - 1;
}
```

If we want to find the **left boundary** of positive numbers

```java
if(nums[middle] > 0){
    high = middle - 1;
}
else{
    low = middle + 1;
}
```

```java
public int maximumCount(int[] nums) {
    if(nums[0] > 0){
        return nums.length;
    }
    int positive_start = 0;
    int negative_high = getNegativeBoundary(nums);
    int positive_low = getPositiveBoundary(nums);
    return negative_high + 1 > (nums.length - positive_low) ? negative_high + 1 : (nums.length - positive_low);

}
public int getPositiveBoundary(int[] nums){
    int low = 0;
    int high = nums.length - 1;
    while(low <= high){
        int middle = low + (high - low) / 2;
        if(nums[middle] > 0){
            high = middle - 1;
        }
        else{
            low = middle + 1;
        }
    }
    return low;
}

public int getNegativeBoundary(int[] nums){
    int low = 0;
    int high = nums.length - 1;
    while(low <= high){
        int middle = low + (high - low) / 2;
        if(nums[middle] < 0){
            low = middle + 1;
        }
        else{
            high = middle - 1;
        }
    }
    return high; 
}
```
