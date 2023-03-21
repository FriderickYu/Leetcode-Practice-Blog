# Day 44 March twentieth-first Dynamic Programming

## [Leetcode 121 Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)

### Determine dp table and index

`dp[i][0]` represents the maximum money we can get if we possess stock when it's `i` day.

`dp[i][1]` represents the maximum money we can get if we don't possess stock when it's `i` day.

### Determine recursion formula

There are two situations in `dp[i][0]`:

1. We purchase stock before i day, so maintain the current situation, `dp[i][0] = dp[i - 1][0]`
2. We didn't purchase stock before, so purchasing stock when it's `i` day, `dp[i][0] = -prices[i]`

There are two situations in `dp[i][1]`:

1. We sell stock before i day, so maintain the current situation, `dp[i][1] =  dp[i - 1][1]`
2. We sell stock when it's `i` day,`dp[i][1] = prices[i] + dp[i - 1][0]`

### How to initialize dp table

According to recursion formula, `dp[0][0] = -prices[0]`, `dp[0][1] = 0`

### Determine the order of traverse

According to recursion formula, `dp[i]` depends on `dp[i - 1]`

### Derive dp by example:

<img src="../picture/March%20twentieth-first/stock1.jpg" width = "400" height = "420" alt="tree2" align=center/>

```java
public int maxProfit(int[] prices) {
    if(prices == null || prices.length == 0){
        return 0;
    }
    int[][] dp = new int[prices.length][2];
    dp[0][0] = -prices[0];
    dp[0][1] = 0;
    for(int i = 1; i < prices.length; i ++){
        dp[i][0] = Math.max(dp[i - 1][0], -prices[i]);
        dp[i][1] = Math.max(dp[i - 1][1], prices[i] + dp[i - 1][0]);
    }
    return dp[prices.length - 1][1];
}
```

## [Leetcode 122 Best Time to Buy and Sell Stock II](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/)

### Determine dp table and index

`dp[i][0]` represents the maximum money we can get if we possess stock when it's `i` day.

`dp[i][1]` represents the maximum money we can get if we don't possess stock when it's `i` day.

### Determine recursion formula

`dp[i][0]`

1. Possess stock `i - 1` day, maintain the current situation, `dp[i][0] = dp[i - 1][0]`
2. Purchase stock `i` day, so it's previous money we have minus current stock price, `dp[i][0] = dp[i - 1][1] - prices[i]`

`dp[i][1]`

1. We don't possess `i - 1` day, maintain the current situation, `dp[i][1] = dp[i - 1][1]`
2. Sell stock `i` day, `dp[i][1] = dp[i - 1][0] + prices[i]`

```java
public int maxProfit(int[] prices) {
    int[][] dp = new int[prices.length][2];
    dp[0][0] = -prices[0];
    dp[0][1] = 0;
    for(int i = 1; i < prices.length; i ++){
        dp[i][0] = Math.max(dp[i - 1][0], dp[i - 1][1] - prices[i]);
        dp[i][1] = Math.max(dp[i - 1][1], dp[i - 1][0] + prices[i]);
    }
    return dp[prices.length - 1][1];
}
```