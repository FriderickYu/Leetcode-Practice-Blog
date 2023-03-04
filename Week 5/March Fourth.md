# Day 30 March Fourth Greedy

## [Leetcode 122 Best Time to Buy and Sell Stock II](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/description/)


| Day1 | Day2 | Day3 | Day4 | Day5 | Day6 |
| ------ | ------ | ------ | ------ | ------ | ------ |
| 7    | 1    | 5    | 3    | 6    | 4    |
|      | -6   | 4    | -2   | 3    | -2   |

As the table shows, 7 days. The stock will only produce value(positive or negative) from the day2.

The only thing we need to do is to count when the profit is positive, and accumulate it.

```java
public int maxProfit(int[] prices) {
    int result = 0;
    for(int i = 1; i < prices.length; i ++){
        if(prices[i] - prices[i - 1] <= 0){
            continue;
        }
        else{
            result += prices[i] - prices[i - 1];
        }
    }
    return result;
}
```

## [Leetcode 55 Jump Game](https://leetcode.com/problems/jump-game/description/)

Don't attempt to choose every possiblility(move one or others). When move to a step, cover the range as much as possible, find whether finally end will be involved in the range or not

```java
public boolean canJump(int[] nums) {
    int coverRange = 0;
    for(int i = 0; i <= coverRange; i ++){
        coverRange = Math.max(coverRange, i + nums[i]);
        if(coverRange >= nums.length - 1){
            return true;
        }
    }
    return false;
}
```

## [Leetcode 45 Jump Game II](https://leetcode.com/problems/jump-game-ii/description/)

```java
public int jump(int[] nums) {
    if(nums == null || nums.length == 0 || nums.length == 1){
        return 0;
    }
    int count = 0;
    int curRange = 0;
    int maxRange = 0;
    for(int i = 0; i < nums.length; i ++){
        maxRange = Math.max(maxRange, i + nums[i]);
        if(maxRange >= nums.length - 1){
            count ++;
            break;
        }
        if(i == curRange){
            curRange = maxRange;
            count ++;
        }
    }
    return count;
}
```
