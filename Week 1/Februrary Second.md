# Day 2 February 2nd

## Leetcode 977 Squares of a Sorted Array

[Video Analysis](https://www.bilibili.com/video/BV1QB4y1D7ep/?vd_source=e8f9779956746463d5471d7c18ccae92)

Including:

- Arrays
- Built-in function
- Double pointers

### Purely Built-in

Just simply square every element in the `array`, and using built-in sort `Arrays.sort()`

```java
for(int i = 0; i < nums.length; i ++)
{
    nums[i] = (int)Math.pow(nums[i], 2);
}
Arrays.sort(nums);
return nums;
```

It is easy to implement, but not to consider the time complexity

### Double Pointers

`[-4, -1, 0, 3, 10]`

If you carefully observe the above array, you may find that it is not a entirely disordered

At the first part, `[-4, -1, 0]`, it is descending, however at the second part, it is ascending

Therefore, elements are at both ends, could be the bigger element, comparing them, if one’s square is bigger, putting it into `array`

![Untitled](../picture/Februrary%20Second/Untitled.jpeg)

```java
int[] array = new int[nums.length];
int put_index = array.length - 1;
int i = 0;
int j = nums.length - 1;
while(i <= j){
    if(nums[i] * nums[i] > nums[j] * nums[j]){
        array[put_index] = nums[i] * nums[i];
        i ++;
    }
    else{
        array[put_index] = nums[j] * nums[j];
        j --;
    }
    put_index --;
}
return array;
```

Time Complexity is `O(n)`

## Leetcode **209 Minimum Size Subarray Sum**

[Video Analysis](https://www.bilibili.com/video/BV1tZ4y1q7XE/?spm_id_from=333.788&vd_source=e8f9779956746463d5471d7c18ccae92)

Including:

- Array
- Double Pointers
- Sliding Window

This question can be solved in brute-force, basically a nested loop consisting of two loops, however it is hard to implement, plus time complexity will be $O(n^2)$

Therefore, sliding window is a good option. Basically, adjusting start and end positions of subsequences via double pointers, only one loop is required.

![Untitled](../picture/Februrary%20Second/Untitled%201.jpeg)

At the beginning, begin and end pointers point to the initial element

![Untitled](../picture/Februrary%20Second/Untitled%202.jpeg)

Then, moving the end pointer, accumulating the sum, until the sum is larger than target, and do not forget record the length

![Untitled](../picture/Februrary%20Second/Untitled%203.jpeg)

Once we find a sum that is larger than target, start to move begin pointer, finding whether there are subarray bigger than target, and do not forget reduce from the sum. If exist, record the length, and keeping moving begin pointer, if not start to move end pointer

![Untitled](../picture/Februrary%20Second/Untitled%204.jpeg)

![Untitled](../picture/Februrary%20Second/Untitled%205.jpeg)

![Untitled](../picture/Februrary%20Second/Untitled%206.jpeg)

```java
int sum = 0;
int begin = 0;
int result = Integer.MAX_VALUE;
for(int end = 0; end < nums.length; end ++){
    sum += nums[end];
    while(sum >= target){
        result = Math.min(result, end - begin + 1);
        sum -= nums[begin];
        begin ++;
    }
}
return result == Integer.MAX_VALUE ? 0 : result;
```