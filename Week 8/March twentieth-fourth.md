# Day 47 March twentieth-fourth Dynamic Programming

## [Leetcode 300 Longest Increasing Subsequence](https://leetcode.com/problems/longest-increasing-subsequence/description/)

### Determine dp table and index

`dp[i]` represents the length of longest increasing subsequence of nums[i].

This picture illustrates the basic idea

<img src="../picture/March%20twentieth-fourth/comparsion.jpg" width = "400" height = "200" alt="comparsion" align=center/>

Naturally, we wanna compare two numbers to ensure increasing order, like `nums[i]` and `nums[j]`, so there are two subsequences which end with `nums[i]` and `nums[j]`.

### Determine recursion formula

When we traverse to `nums[i]`, surely we need to compare it with all numbers from 0 -> i -1, if `nums[i] > nums[j]`, then `dp[i] = max(dp[i], dp[j] + 1)`

### How to initialize dp table

A single number is also an increasing subsequence, then we can initialize the array to 1, therefore `dp[0]` is also 1.

### Determine the order of traverse

`dp[i]` depends on previous nunber, so the order of traverse is from start to end.

### Derive dp by example:

<img src="../picture/March%20twentieth-fourth/example1.jpg" width = "400" height = "400" alt="example1" align=center/>

```java
public int lengthOfLIS(int[] nums) {
    int[] dp = new int[nums.length];
    Arrays.fill(dp, 1);
    for(int i = 1; i < nums.length; i ++){
        for(int j = 0; j < i; j ++){
            if(nums[i] > nums[j]){
                dp[i] = Math.max(dp[i], dp[j] + 1);
            }
        }
    }
    int result = 0;
    for(int i = 0; i < dp.length; i ++){
        result = Math.max(result, dp[i]);
    }
    return result;
}
```

## [Leetcode 674 Longest Continuous Increasing Subsequence](https://leetcode.com/problems/longest-continuous-increasing-subsequence/description/)

This is a very easy question. Because the answer requires *continuous* subsequence. So we only need to compare adjacent elements, `if(nums[i] > nums[i - 1])`, then `dp[i] = dp[i - 1] + 1`

```java
public int findLengthOfLCIS(int[] nums) {
    int[] dp = new int[nums.length];
    Arrays.fill(dp, 1);
    for(int i = 1; i < nums.length; i ++){
        if(nums[i] > nums[i - 1]){
            dp[i] = dp[i - 1] + 1;
        }
    }
    int max = 0;
    for(int i = 0; i < dp.length; i ++){
        if(dp[i] > max){
            max = dp[i];
        }
    }
    return max;
}
```
