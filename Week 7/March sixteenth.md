# Day 40 March Sixteenth Dynamic Programming

## Complete Knapsack Problem

Description: There are `n` items, and a knapsack that can carry a maximum weight of `w`. The weight of  $i^{th}$ is `weight[i]`, its value is `value[i]`. Each item **can be used for multiple times, or none**. Solving for which items will be loaded into the knapsack with **the greatest sum of values**.

Taking an expample to illustrate the idea, a knapsack, its maximum capacity is 4


|        | Weight | Value |
| -------- | -------- | ------- |
| Item 0 | 1      | 15    |
| Item 1 | 3      | 20    |
| Item 2 | 4      | 30    |

Almost all steps of complete knapsack are similar with 01 knapsack, except the order of traversal. In the previous 01 knapsack, it's a two nested loop. First is item, then capacity.

The reason why we traverse the capacity from end to start, is that we wanna guarantee each item will be only used for once

<img src="../picture/March%20sixteenth/original_01dp.jpg" width = "400" height = "160" alt="original" align=center/>

```java
for(int i = 0; i < weight.length; i ++){
    for(int j = bagSize; j >= weight[i]; j --){
        dp[j] = Math.max(dp[j], dp[j - weight[i]] + value[i]);
    }
}
```

If we change the order of traversal, from start to end, then it comes to:

<img src="../picture/March%20sixteenth/startToEnd.jpg" width = "400" height = "190" alt="original" align=center/>

```java
for(int i = 0; i < weight.length; i ++){
    for(int j = weight[i]; j <= bagSize; j ++){
        dp[j] = Math.max(dp[j], dp[j - weight[i]] + value[i]);
    }
}
```

```java
private static void CompleteKnapsack(){
    int[] weight = {1, 3, 4};
    int[] value = {15, 20, 30};
    int bagWeight = 4;
    int[] dp = new int[bagWeight + 1];
    for (int i = 0; i < weight.length; i++){
        for (int j = weight[i]; j <= bagWeight; j++){
            dp[j] = Math.max(dp[j], dp[j - weight[i]] + value[i]);
        }
    }
    for (int maxValue : dp){
        System.out.println(maxValue + "   ");
    }
}
```
