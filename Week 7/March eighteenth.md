# Day 42 March eighteenth Dynamic Programming

## [Leetcode 139 Word Break](https://leetcode.com/problems/word-break/)

### Determine dp table and index

`dp[i]` represents if the length of string is i, and `dp[i]` is true, then means from 0 - i, this substring is a string that can be divided.

### Determine recursion formula

```java
if(dp[j] == true){
    // j < i
    if(dict.contains(string.substring(j, i))){
        dp[i] = true;
    }
}
```

So `(dp[j] = true && wordDict.contains(s.substring(j, i)))`

### How to initialize dp table

Considering `j < i`, `dp[i]` depends on `dp[j]`, so `dp[0]` is the foundation. Therefore, `dp[0]` should be true

### Determine the order of traverse

```java
s = "applepenapple"
wordDict = ["apple","pen"]
```

Therefore, this is a permutation problem.

* In a permutation problem, first loop is knapsack, the second loop is item
* In a combination problem, first loop is item, the second loop is knapsack

### Derive dp by example:

<img src="../picture/March%20eighteenth/derivation_example.jpg" width = "400" height = "302" alt="derivation_example" align=center/>

```java
public boolean wordBreak(String s, List<String> wordDict) {
    boolean[] dp = new boolean[s.length() + 1];
        dp[0] = true;
    for (int i = 1; i <= s.length(); i++) {
        for (int j = 0; j < i && !dp[i]; j++) {
            if (wordDict.contains(s.substring(j, i)) && dp[j]) {
                dp[i] = true;
            }
        }
    }

    return dp[s.length()];
}
```

## Multiple Knapsack Problem
