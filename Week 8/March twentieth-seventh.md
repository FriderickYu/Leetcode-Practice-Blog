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
