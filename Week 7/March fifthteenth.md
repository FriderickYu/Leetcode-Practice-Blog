# Day 39 March fifthteenth Dynamic Programming

## [Leetcode 1049 Last Stone Weight II](https://leetcode.com/problems/last-stone-weight-ii/description/)

This question is almost the same with [Leetcode 416 Partition Equal Subset Sum](https://leetcode.com/problems/partition-equal-subset-sum/)

<img src="../picture/March%20fifthteenth/01dp1.jpg" width = "400" height = "250" alt="tree" align=center/>

This question can be abstracted to a 01 knapsack problem. We first accumulate the sum of this array, then divided it by 2. The purpose is to find which numbers can be consisted to the `sum / 2`, if not, as close as possible.

### Determine dp table and index

`dp[j]` represents the maximum value when capacity is j. In this question, `weight[i]` and `value[i]` are `stones[i]`

### Determine recursion formula

`dp[j] = max(dp[j], dp[j - stones[i]] + stones[i]`

### How to initialize dp table

When capacity is 0, the maximum value should also be 0

```java
public int lastStoneWeightII(int[] stones) {
    int sum = 0;
    for(int stone : stones){
        sum += stone;
    }
    int target = sum / 2;
    int[] dp = new int[target + 1];
    dp[0] = 0;
    for(int i = 0; i < stones.length; i ++){
        for(int j = target; j >= stones[i]; j --){
            dp[j] = Math.max(dp[j], dp[j - stones[i]] + stones[i]);
        }
    }
    return sum - dp[target] - dp[target];
}
```

## [Leetcode 494 Target Sum](https://leetcode.com/problems/target-sum/submissions/915566057/)

There are two signals, `+` and `-`, so in this array, there will be two subarrays, plus array(left), and minus array(right). So plus + minus is equal to sum of array. And plus - minus is equal to target. Therefore:

$left + right = sum$

$left - right = target$

$left = \frac{sum + target}{2}$

If we select certain elements from the array, which guarantees that the sum of these elements are equal to `left`, then convert it to a 01 knapsack problem.

### Determine dp table and index

`dp[j]` means to reach capacity j, there are `dp[j` ways

### Determine recursion formula

For instance, for `nums[i]`, if we wanna reach `dp[j]` there are `dp[j - nums[i]]`

Means, if capacity is 5, and `nums[i]` is 1, which means the rest capacity is 4, and we need to focus on the value of `dp[4]`

So, `dp[j] += dp[j - nums[i]]`

### How to initialize dp table

According to the recursion formula, we know `dp[0]` should be 1

```java
public int findTargetSumWays(int[] nums, int target) {
    int sum = 0;
    for(int num : nums){
        sum += num;
    }
    if(target < 0 && sum < -target){
        return 0;
    }
    if((target + sum) % 2 != 0){
        return 0;
    }
    int size = (target + sum) / 2;
    if(size < 0){
        size = -size;
    }
    int[] dp = new int[size + 1];
    dp[0] = 1;
    for(int i = 0; i < nums.length; i ++){
        for(int j = size; j >= nums[i]; j --){
            dp[j] += dp[j - nums[i]];
        }
    }
    return dp[size];
}
```

## [Leetcode 474 Ones and Zeroes](https://leetcode.com/problems/ones-and-zeroes/)
