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

## [Leetcode 96 Unique Binary Search Trees](https://leetcode.com/problems/unique-binary-search-trees/)

<img src="../picture/March%20Thirdteenth/tree.jpg" width = "400" height = "380" alt="tree" align=center/>

n is 3:

* when 1 is the root, there are two elements on the right tree. The layout is the same as when n is 2
* when 3 is the root, there are two elements on the left tree. The layout is the same as when n is 2

So it is not hard to find that:

* The number of seach tree when 1 is the root = the number of search tree when there are 2 elements on the right * the number of search tree when there are 0 elements on the left
* The number of search tree when 2 is the root = the number of search tree when there is 1 element on the right * the number of search tree when there is 1 element on the left
* The number of search tree when 3 is the root = the number of search tree when there are 0 elements on the right * the number of search tree when there are 2 elements on the left

### Determine dp table and index

`dp[i]` means the number of binary search tree when `1 - i` is the root

### Determine recursion formula

`dp[i]` += `dp[the number of nodes of left tree when j is the root]` * `dp[the number of nodes of right tree when j is the root]`

`dp[i] = dp[j - 1] * dp[i - j]`

### How to initialize dp table

From the definition, when root is null, it's also a binary tree.

From the recursion formula side, `dp[i] = dp[j - 1] * dp[i - j]`, we should not also either side is 0, so `dp[0]

### Determine the order of traverse

```java
public int numTrees(int n) {
    int[] dp = new int[n + 1];
    dp[0] = 1;
    dp[1] = 1;
    for(int i = 2; i <= n; i ++){
        for(int j = 1; j <= i; j ++){
            dp[i] += dp[j - 1] * dp[i - j];
        }
    }
    return dp[n];
}
```
