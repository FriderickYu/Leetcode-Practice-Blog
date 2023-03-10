# Day 35 March Tenth Dynamic Programming

If a problem has many overlapping sub-problems, it is most efficient to use dynamic programming. Because each state in dynamic programming must be derived from the previous state, which distinguishes it from greedy, which has no state derivation but chooses the local optimum instead.

Five steps you should know in dynamic programming:

1. determine dp table and index
2. determine recursion formula
3. how to initialize dp table
4. determine the order of traverse
5. derive dp table by example

## [Leetcode 509 Fibonacci Number](https://leetcode.com/problems/fibonacci-number/description/)

We will solve this problem by five steps, first after analyzing the problem, we knew dp is suitable for this problem, because each state is derived from the previous state

1. determine dp table and index: dp[i] stores the Fibonacci value of $i^{th}$
2. determine recursion formula: `F(n) = F(n - 1) + F(n - 2), for n > 1`
3. how to initialize dp table: `F(0) = 0, F(1) = 1`
4. determine the order of traverse: because `F(n) = F(n - 1) + F(n - 2), for n > 1`, we know that the Fibonacci value of $i^{th}$ depends on previous two numbers, so the traverse order should be from start to end
5. derive dp table by example: `dp[10] = 55`, sometimes simply print dp table will be a good choice to debug

```java
fib[2] = 1
fib[3] = 2
fib[4] = 3
fib[5] = 5
fib[6] = 8
fib[7] = 13
fib[8] = 21
fib[9] = 34
fib[10] = 55
```

```java
public int fib(int n) {
    if(n <= 1){
        return n;
    }
    int[] dp = new int[n + 1];
    dp[0] = 0;
    dp[1] = 1;
    for(int i = 2; i <= n; i ++){
        dp[i] = dp[i - 1] + dp[i - 2];
    }
    return dp[n];
}
```

## [Leetcode 70 Climbing Stairs](https://leetcode.com/problems/climbing-stairs/description/)

So, deducing some examples and it's not difficult to find that `dp[i]`, it's `dp[i - 1] + one step` and `dp[i - 2] + two steps`, therefore `dp[i] = dp[i - 1] + dp[i - 2]`. The each state can be derived from previous states, so this is a dp problem.

1. determine dp table and index: dp[i] stores how many distinct ways in can we climb to i steps
2. determine recursion formula: `dp[i] = dp[i - 1] + dp[i - 2], for i > 2`
3. how to initialize dp table: `dp[1] = 1, dp[2] = 2`
4. determine the order of traverse: according to step 2, it's from start to end
5. derive dp by example: `dp[4] = 5`

```java
public int climbStairs(int n) {
    if(n <= 2){
        return n;
    }
    int[] dp = new int[n + 1];
    dp[1] = 1;
    dp[2] = 2;
    for(int i = 3; i <= n; i ++){
        dp[i] = dp[i - 1] + dp[i - 2];
    }
    return dp[n];
}
```
