# Day 37 March Thirdteenth Dynamic Programming

## [Leetcode 343 Integer Break](https://leetcode.com/problems/integer-break/description/)

### Determine dp table and index

`dp[i]` means, the maximum product that can be obtained after splitting the number `i`

### Determine recursion formula

There are two ways we can obtain the result, suppose when we iterate from 1 to j, then

* Two numbers, `dp[i] = j * (i - j);`
* At least two numbers, `dp[i] = j * dp[i - j];`

### How to initialize dp table

Mp need to confuse about `dp[0]` and `dp[1]`. Start with `dp[2] = 1`

### Determine the order of traverse

Both `dp[i] = j * (i - j)` and `dp[i] = j * dp[i - j]` relies on previous states, then just traversing this array from the start to end

```java
public int integerBreak(int n) {
    int[] dp = new int[n + 1];
    dp[2] = 1;
    for(int i = 3; i <= n; i ++){
        for(int j = 1; j <= i - j; j ++){
            dp[i] = Math.max(dp[i], Math.max(j * (i - j), j * dp[i - j]));
        }
    }
    return dp[n];
}
```
