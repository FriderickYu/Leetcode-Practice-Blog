# Day 49 March twentieth-seventh Dynamic Programming

## [Leetcode 392 Is Subsequence](https://leetcode.com/problems/is-subsequence/)

### Determine dp table and index

`dp[i][j]` represents the length of same subsequence of `s` ends with `i - 1`, and `t` ends with `j - 1`

### Determine recursion formula

* `if(s[i - 1] == t[j - 1])`, means we find a character, appears in `t`, also in `s`
* `if(s[i - 1] != t[j - 1])`, means we find a character, appears in `s`, doesn't appear in `t`

If `(s[i - 1] == t[j - 1])`, then `dp[i][j] = dp[i - 1][j - 1] + 1`

But if `(s[i - 1] != t[j - 1])`, which means we need to *delete* this character in `t`, then remaining the result of `s[i - 1]` and `t[j - 2]`

### How to initialize dp table

`dp[i][0]`, `dp[0][j]` and `dp[0][0]` should be 0

### Determine the order of traverse

From start to end

### Derive dp by example:

<img src="../picture/March%20twentieth-seventh/sequence.jpg" width = "400" height = "270" alt="sequence" align=center/>

```java
public boolean isSubsequence(String s, String t) {
    int[][] dp = new int[s.length() + 1][t.length() + 1];
    for(int i = 1; i <= s.length(); i ++){
        for(int j = 1; j <= t.length(); j ++){
            if(s.charAt(i - 1) == t.charAt(j - 1)){
                dp[i][j] = dp[i - 1][j - 1] + 1;
            }
            else{
                dp[i][j] = dp[i][j - 1];
            }
        }
    }
    if(dp[s.length()][t.length()] == s.length()){
        return true;
    }
    else{
        return false;
    }
}
```

## [Leetcode 115 Distinct Subsequences](https://leetcode.com/problems/distinct-subsequences/)

### Determine dp table and index

`dp[i][j]` represents how many same subsequence in `t` belongs to `s`, ends with `i - 1` in `s`, and ends with `j - 1` in `t`

### Determine recursion formula

There are two situations:

1. when `s[i - 1]` is equal with `t[j - 1]`
2. when `s[i - 1]` is not equal with `t[j - 1]`

And when `s[i - 1]` is equal with `t[j - 1]`, it can also divide into two sub-situations:

1. Using `s[i - 1]` to match, then `dp[i - 1][j - 1]`
2. We can also, don't use `s[i - 1]` to match, `dp[i - 1][j]`

The reason why we don't use `s[i - 1]` to match, for instance, `s = bagg` and `t = bag`, `s[3]` is equal with `s[2]`, g is repeated for two times in `s`

So `dp[i][j] = dp[i - 1][j - 1] + dp[i - 1][j]`

When `s[i - 1]` is not equal with `t[j - 1]`, `dp[i][j] = dp[i - 1][j]`

### How to initialize dp table

`dp[i][j]` really depends on `dp[i - 1][j - 1]` and `dp[i - 1][j]`. `dp[i][0]` is very special, when `t` is null, `s` contains 1 of `t`

### Determine the order of traverse

### Derive dp by example:

<img src="../picture/March%20twentieth-seventh/distinct_subsequence.jpg" width = "400" height = "430" alt="distinct_subsequence" align=center/>

```java
public int numDistinct(String s, String t) {
    int[][] dp = new int[s.length() + 1][t.length() + 1];
    for(int i = 0; i <= s.length(); i ++){
        dp[i][0] = 1;
    }
    for(int i = 1; i <= s.length(); i ++){
        for(int j = 1; j <= t.length(); j ++){
            if(s.charAt(i - 1) == t.charAt(j - 1)){
                dp[i][j] = dp[i - 1][j - 1] + dp[i - 1][j];
            }
            else{
                dp[i][j] = dp[i - 1][j];
            }
        }
    }
    return dp[s.length()][t.length()];
}
```
