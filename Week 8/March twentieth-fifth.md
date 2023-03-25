## Day 48 March twentieth-fifth Dynamic Programming

## [Leetcode 1143 Longest Common Subsequence](https://leetcode.com/problems/longest-common-subsequence/description/)

### Determine dp table and index

`dp[i][j]` represents the length of longest common subsequence, of both `[0, i - 1]` text1 and `[0, j - 1]` text2

### Determine recursion formula

There are two situations:

1. When `text1[i - 1]` is equal with `text2[j - 1]`, then we find something in common, `dp[i][j] = dp[i - 1][j - 1] + 1`
2. When `text1[i - 1]` is not equal with `text2[j - 1]`, then we should find the longest common subsequence of `text1[0, i - 2]` and `text2[0, j - 1]`, or `text1[0, i - 1]` and `text2[0, j - 2]`, take the maximum

### How to initialize dp table

`dp[i][0] = 0` and `dp[0][j] = 0`

### Determine the order of traverse

`dp[i][j]` depends on `dp[i - 1][j -1]`, `dp[i - 1][j]` and `dp[i][j - 1]`, so from start to end

### Derive dp by example:

<img src="../picture/March%20twentieth-fifth/example1.jpg" width = "400" height = "420" alt="example1" align=center/>

```java
public int longestCommonSubsequence(String text1, String text2) {
    int[][] dp = new int[text1.length() + 1][text2.length() + 2];
    for(int i = 1; i <= text1.length(); i ++){
        char c1 = text1.charAt(i - 1);
        for(int j = 1; j <= text2.length(); j ++){
            char c2 = text2.charAt(j - 1);
            if(c1 == c2){
                dp[i][j] = dp[i - 1][j - 1] + 1;
            }
            else{
                dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]);
            }
        }
    }
    return dp[text1.length()][text2.length()];
}
```

## [Leetcode 1035 Uncrossed Lines](https://leetcode.com/problems/uncrossed-lines/description/)

This question is the same with [Leetcode 1143 Longest Common Subsequence](https://leetcode.com/problems/longest-common-subsequence/description/). The question is how to abstract this question.

<img src="../picture/March%20twentieth-fifth/uncrossed_line.jpg" width = "400" height = "350" alt="uncrossed_line" align=center/>

We don't wanna include crossed lines, so this is just another version of the longest common subsequence

```java
public int maxUncrossedLines(int[] nums1, int[] nums2) {
    int[][] dp = new int[nums1.length + 1][nums2.length + 2];
    for(int i = 1; i <= nums1.length; i ++){
        for(int j = 1; j <= nums2.length; j ++){
            if(nums1[i - 1] == nums2[j - 1]){
                dp[i][j] = dp[i - 1][j - 1] + 1;
            }
            else{
                dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]);
            }
        }
    }
    return dp[nums1.length][nums2.length];
}
```

## [Leetcode 53 Maximum Subarray](https://leetcode.com/problems/maximum-subarray/description/)

### Determine dp table and index

`dp[i]` represents the maximum sum of continous subarray, ends with `nums[i]`

### Determine recursion formula

`dp[i]` can be calculated by two ways:

1. `dp[i] = dp[i - 1] + nums[i]`
2. `nums[i]`

### How to initialize dp table

From the recursion formula, `dp[0] = nums[0]`

### Determine the order of traverse

`dp[i]` depends on `nums[i]` and `dp[i - 1]`

### Derive dp by example:

<img src="../picture/March%20twentieth-fifth/maximum_subarray.jpg" width = "400" height = "228" alt="maximum_subarray" align=center/>

```java
public int maxSubArray(int[] nums) {
    if(nums.length == 1){
        return nums[0];
    }
    int[] dp = new int[nums.length];
    dp[0] = nums[0];
    for(int i = 1; i < nums.length; i ++){
        dp[i] = Math.max(dp[i - 1] + nums[i], nums[i]);
    }
    int max = Integer.MIN_VALUE;
    for(int j = 0; j < nums.length; j ++){
        if(dp[j] > max){
            max = dp[j];
        }
    }
    return max;
}
```
