# Day 50 March twentieth-eighth Dynamic Programming

## [Leetcode 583 Delete Operation for Two Strings](https://leetcode.com/problems/delete-operation-for-two-strings/)

### Determine dp table and index

`dp[i][j]` represents how many deletion actions we should do to let `word1` is equal with `word2`, ends with `i - 1` of `word1` and `j - 1` of `word2`

### Determine recursion formula

There are two situations:

1. `word1[i - 1]` is equal with `word2[j - 1]`
2. `word1[i - 1]` is not equal with `word2[j - 1]`

When they are equal, like `sea` and `eat`, when `i = 3`, and `j = 2`, it is the same with when `i = 2` and `j = 1`

The circumstance is quiet different if they are not equal.

1. We choose delete `word1[i - 1]`, so it's `dp[i - 1][j] + 1`
2. We choose delete `word2[j - 1]`, so it's `dp[i][j - 1] + 1`
3. Deleting `word1[i - 1]` and `word2[j - 1]`, so it's dp[i - 1][j - 1] + 2

`dp[i][j] = min(dp[i - 1][j] + 1, dp[i][j - 1] + 1, dp[i - 1][j - 1] + 2)`

### How to initialize dp table

According to the recursion formula, we know we should initialize `dp[0][0]`, `dp[i][0]` and `dp[0][j]`

Like when `word1 = eat` and `word2 = null`, it needs three times of deletion to cut `word1` to null, so `dp[i][0] = i` and `dp[0][j] = j`, `dp[0][0] = 0`

### Determine the order of traverse

From top to end, from left to right

### Derive dp by example:

<img src="../picture/March%20twentieth-eighth/example1.jpg" width = "400" height = "430" alt="example1" align=center/>

```java
public int minDistance(String word1, String word2) {
    int[][] dp = new int[word1.length() + 1][word2.length() + 1];
    dp[0][0] = 0;
    for(int i = 1; i <= word1.length(); i ++){
        dp[i][0] = i;
    }
    for(int j = 1; j <= word2.length(); j ++){
        dp[0][j] = j;
    }
    for(int i = 1; i <= word1.length(); i ++){
        for(int j = 1; j <= word2.length(); j ++){
            if(word1.charAt(i - 1) == word2.charAt(j - 1)){
                dp[i][j] = dp[i - 1][j - 1];
            }
            else{
                dp[i][j] = Math.min(dp[i - 1][j - 1] + 2, Math.min(dp[i - 1][j] + 1, dp[i][j - 1] + 1));
            }
        }
    }
    return dp[word1.length()][word2.length()];
}
```

## [Leetcode 72 Edit Distance](https://leetcode.com/problems/edit-distance/)

### Determine dp table and index

`dp[i][j]` means the minimum edit distance, which ends with `i - 1` of `word1` and `j - 1` of `word2`

### Determine recursion formula

```java
if(word1[i - 1] == word2[j - 1]){
    // none
}
if(word1[i - 1] != word2[j - 1]){
    // insert
    // delete
    // replace
        }
```

`if(word1[i - 1] == word2[j - 1])` means we do nothing, `dp[i][j] = dp[i - 1][j - 1]`

There are many circumstances of `if(word1[i - 1] != word2[j - 1])`

1. we delete an element of `word1`, then `dp[i][j] = dp[i - 1][j] + 1`
2. we delete an element of `word2`, then `dp[i][j] = dp[i][j - 1] + 1`
3. Actually, insertion is another kind of deletion. For example, `word1 = ad` and `word2 = a`, delete `d` in `word1`, or insert `d` in `word2`, it's the same thing
4. Replace an element, it's similar with `if(word1[i - 1] == word2[j - 1])`, except we have to `+1`

So, `dp[i][j] = min(dp[i - 1][j - 1] + 1, min(dp[i][j - 1] + 1, dp[i - 1][j] + 1));`

### How to initialize dp table

Same with [Leetcode 583 Delete Operation for Two Strings](https://leetcode.com/problems/delete-operation-for-two-strings/)

### Determine the order of traverse

Same with [Leetcode 583 Delete Operation for Two Strings](https://leetcode.com/problems/delete-operation-for-two-strings/)

### Derive dp by example:

<img src="../picture/March%20twentieth-eighth/example2.jpg" width = "400" height = "430" alt="example2" align=center/>

```java
public int minDistance(String word1, String word2) {
    int[][] dp = new int[word1.length() + 1][word2.length() + 1];
    dp[0][0] = 0;
    for(int i = 1; i <= word1.length(); i ++){
        dp[i][0] = i;
    }
    for(int j = 1; j <= word2.length(); j ++){
        dp[0][j] = j;
    }
    for(int i = 1; i <= word1.length(); i ++){
        for(int j = 1; j <= word2.length(); j ++){
            if(word1.charAt(i - 1) == word2.charAt(j - 1)){
                dp[i][j] = dp[i - 1][j - 1];
            }
            else{
                dp[i][j] = Math.min(Math.min(dp[i - 1][j] + 1, dp[i][j - 1] + 1), dp[i - 1][j - 1] + 1);
            }
        }
    }
    return dp[word1.length()][word2.length()];
}
```
