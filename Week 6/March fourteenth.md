# Day 38 March fourteenth Dynamic Programming, Knapsack problem

Knapsack problem is a typical DP problem, according to different situation, it can be classified to:

1. *01 Knapsack*
2. *Complete Knapsack*
3. Multiple Knapsack
4. Group Knapsack
5. Hybrid Knapsack

<img src="../picture/March%20fourteenth/knapsack.jpg" width = "500" height = "175" alt="knapsack classification" align=center/>

## 01 Knapsack Problem

Description: There are `n` items, and a knapsack that can carry a maximum weight of `w`. The weight of  $i^{th}$ is `weight[i]`, its value is `value[i]`. Each item is only used once. Solving for which items will be loaded into the knapsack with **the greatest sum of values**.

Brute-force: for each item, there are only two states, pick it or not, therefore backtracking is an optional, time complexity will be $O(2^n)$, n represents the number of items.

Taking an expample to illustrate the idea, a knapsack, its maximum capacity is 4


|        | Weight | Value |
| -------- | -------- | ------- |
| Item 0 | 1      | 15    |
| Item 1 | 3      | 20    |
| Item 2 | 4      | 30    |

*What is the maximum value of items that can be carried in the knapsack?*

### Determine dp table and index

One way is to use a two-dimensional array, `dp[i][j]` means the maximum value of the items taken from the index `[0 - i]`, and put them into a knapsack with capacity `j`

<img src="../picture/March%20fourteenth/meaning%20of%20dp.jpg" width = "400" height = "225" alt="meaning of dp" align=center/>

### Determine recursion formula

According to  the definition of `dp`, `dp[i][j]` represents the maximum value of weights, which we select from 0 to i. For instance, `dp[2][3]` means the maximum value, which we select from item 0 to item 2, and the knapsack capacity is 3. So if we just consider one element, there are two options:

1. don't pick it: If the capacity is smaller than the weight of item, then it should be `dp[i - 1][j]`, because the maximum value should be identical with `dp[i - 1][j]` if we don't pick item i. For example, if we don't pick item 2 when capacity is 3, the value should be same with `dp[1][3]`
2. pick it: if we pick this item, then plus `value[i]` first. Previous elements should be `dp[i - 1][j - weight[i]]`, so it's `dp[i - 1][j - weight[i]] + value[i]`

We choose the max value of these two situation finally.

### How to initialize dp table

<img src="../picture/March%20fourteenth/initialization.jpg" width = "400" height = "235" alt="initialization" align=center/>

The most easiest way to think about initialization is the first row and column, just like the above picture. When capacity is 0, then `dp[i][0] == 0`

### Determine the order of traverse

The order of traverse is determined by derivation function

Derivation function:

* `dp[i - 1][j]`
* `dp[i - 1][j - weight[i]] + value[i]`

<img src="../picture/March%20fourteenth/traverse%20order.jpg" width = "400" height = "235" alt="initialization" align=center/>

So we can see that, dp value depends on top value, and upper left directions <font color="yellow">(yellow colour)</font>

<font color="red">The red arrow </font> indicates we traverse item first, and then capacity

<font color="blue">The blue arrow </font> indicates we traverse capacity first, and then item

These two kinds of traversal are both useful

### Derive dp by example:

See the previous picture

```java
public static void main(String[] args) throws IOException {
    int[] weight = {1,3,4};
    int[] value = {15,20,30};
    int bagSize = 4;
    testWeightBagProblem(weight,value,bagSize);
}
public static void testWeightBagProblem(int[] weight, int[] value, int bagSize){
    int[][] dp = new int[weight.length][bagSize + 1];
    /* initialize dp
    * begin with the weight of first item
    * the rest of dp should be same value
    * but before this weight, all should be 0
    * */
    for(int j = weight[0]; j <= bagSize; j ++){
        dp[0][j] = value[0];
    }
    for(int i = 1; i < weight.length; i ++){
        for(int j = 1; j <= bagSize; j ++){
            if(j < weight[i]){
                dp[i][j] = dp[i - 1][j];
            }
            else{
                dp[i][j] = Math.max(dp[i - 1][j], dp[i - 1][j - weight[i]] + value[i]);
            }
        }
    }
    for(int i = 0; i < weight.length; i ++){
        for(int j = 0; j <= bagSize; j ++){
            System.out.print(dp[i][j] + "\t");
        }
        System.out.println("\n");
    }
}
```

## Improvement

If we observe the derivation function carefully, we may find that `dp[i][j] = max(dp[i - 1][j], dp[i - 1][j - weight[i]] + value[i]);`  We can use `dp[i - 1]` to cover up `dp[i]`. Then it comes to `dp[i][j] = max(dp[i][j], dp[i][j - weight[i]] + value[i]);`

It means we can just use one dimensional array instead.

### Determine dp table and index

`dp[j]` represents that the maximum value when the knapsack capacity is `j`

### Determine recursion formula

`dp[j]` can be derived from `dp[j - weight[i]]`, `dp[j - weight[i]]` means the maximum value of `j - weight[i]` knapsack capacity. `dp[j - weight[i]] + value[i]` means `j - weight[i]` knapsack capacity plus the value of item i

So, two options:

1. don't pick it: `dp[j]`, same with `dp[i - 1][j]`
2. pick it: `dp[j - weight[i]] + value[i]`

### How to initialize dp table

When capacity is 0, then the maximum should also be 0; `dp[0] = 0`

### Determine the order of traverse

We also need a two nested-loop here, first is the item, then capacity. However, the difference is, the capacity loop is from end to start, **not start to end**. That is because we wanna ensure that one item is only used once.

For instance, if we traverse from start to end, then `dp[1] = dp[1-0] + 15 = 15`, `dp[2] = dp[2-1]  + 15 = 30`. See, item 0 counts twice. But if we traverse from end to start, then `dp[2] = dp[2-1] + 15 = 15`, `dp[1] = dp[1-0] + 15 = 15`

### Derive dp by example

<img src="../picture/March%20fourteenth/example%20one%20dimension.jpg" width = "400" height = "400" alt="example" align=center/>

```java
public static void main(String[] args) throws IOException {
    int[] weight = {1,3,4};
    int[] value = {15,20,30};
    int bagSize = 4;
    testWeightBagProblem(weight,value,bagSize);
}
public static void testWeightBagProblem(int[] weight, int[] value, int bagSize){
    int[] dp = new int[bagSize + 1];
    dp[0] = 0;
    for(int i = 0; i < weight.length; i ++){
        for(int j = bagSize; j >= weight[i]; j --){
            dp[j] = Math.max(dp[j], dp[j - weight[i]] + value[i]);
        }
    }
    for(int j = 0; j <= bagSize; j ++){
        System.out.print(dp[j] + " ");
    }
}
```

## [Leetcode 416 Partition Equal Subset Sum](https://leetcode.com/problems/partition-equal-subset-sum/)

This question can be abstracted to a knapsack problem, because we want find whether we can divide the array into two parts, so that the sums of two parts will be equal. It means the sum of one of part is same with the sum of entire array, divided by 2. So knapsack capacity should be `sum / 2`, both `weight[i]` and `value[i]` should be `nums[i]`

### Determine dp table and index

`dp[j]` represents that the maximum value when capacity is j

### Determine recursion formula

`dp[j] = max(dp[j], dp[j - nums[i]] + nums[i])`

### How to initialize dp table

When capacity is 0, we can't put anything in the knapsack, so `dp[0] = 0`

### Determine the order of traverse

The order of traverse is exactly same with 01 knapsack problem

```java
public boolean canPartition(int[] nums) {
    if(nums == null || nums.length == 0){
        return false;
    }
    int sum = 0;
    for(int i = 0; i < nums.length; i ++){
        sum += nums[i];
    }
    if(sum % 2 != 0){
        return false;
    }
    int target = sum / 2;
    int [] dp = new int[target + 1];
    for(int i = 0; i < nums.length; i ++){
        for(int j = target; j >= nums[i]; j --){
            dp[j] = Math.max(dp[j], dp[j - nums[i]] + nums[i]);
        }
    }
    if(dp[target] == target){
        return true;
    }
    return false;
}
```
