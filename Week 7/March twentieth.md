# Day March twentieth Dynamic Programming

## [Leetcode 198 House Robber](https://leetcode.com/problems/house-robber/description/)

### Determine dp table and index

`dp[i]` represents that from house 0 to house i, we can steal at most `dp[i]` money

### Determine recursion formula

When we reach to i room, there are two options, steal or not steal. Plus we need to consider not to alert the police, meanwhile get as much money as we can.

Steal: `dp[i] = dp[i - 2] + nums[i]`, because if we decide to steal i room, we will not steal adjacent room

Not steal: `dp[i] = dp[i - 1]`

### How to initialize dp table

According to recursion formula, we know that we have to initialize `dp[0]` and `dp[1]`

`dp[0] = nums[0]`

`dp[1] = max(nums[0], nums[1])`

### Determine the order of traverse

According to recursion formula, `dp[i]` relies on `dp[i - 1]` and `dp[i - 2]`

### Derive dp by example:


| 0 | 1 | 2  | 3  | 4  |
| --- | --- | ---- | ---- | ---- |
| 2 | 7 | 11 | 11 | 12 |

```java
public int rob(int[] nums) {
    if(nums  == null || nums.length == 0){
        return 0;
    }
    if(nums.length == 1){
        return nums[0];
    }
    int[] dp = new int[nums.length];
    dp[0] = nums[0];
    dp[1] = Math.max(nums[0], nums[1]);
    for(int i = 2; i < nums.length; i ++){
        dp[i] = Math.max(dp[i - 2] + nums[i], dp[i - 1]);
    }
    return dp[nums.length - 1];
}
```

## [Leetcode 213 House Robber II](https://leetcode.com/problems/house-robber-ii/)

This question is very similar with [Leetcode 198 House Robber](https://leetcode.com/problems/house-robber/description/), but it 's a circle rather than a line. So we need to deal with different situations

1. We don't consider the first room and the last one, `1 - nums.length -2`
2. We don't consider the first room `1 - nums.length - 1`
3. We don't consider the last one `0 - nums.length - 2`

Situation 2 and 3 involve situation 1. Therefore we only need to consider situation 2 and 3.

```java
public int rob(int[] nums) {
    if(nums == null || nums.length == 0){
        return 0;
    }
    if(nums.length == 1){
        return nums[0];
    }
    int result1 = robDP(nums, 0, nums.length - 2);
    int result2 = robDP(nums, 1, nums.length - 1);
    return Math.max(result1, result2);
}

public int robDP(int[] nums, int start, int end){
    if(start == end){
        return nums[start];
    }
    int[] dp = new int[nums.length];
    dp[start] = nums[start];
    dp[start + 1] = Math.max(nums[start], nums[start + 1]);
    for(int i = start + 2; i <= end; i ++){
        dp[i] = Math.max(dp[i - 2] + nums[i], dp[i - 1]);
    }
    return dp[end];
}
```

## [Leetcode 337 House Robber III](https://leetcode.com/problems/house-robber-iii/)
