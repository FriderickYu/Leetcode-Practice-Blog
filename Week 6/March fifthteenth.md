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