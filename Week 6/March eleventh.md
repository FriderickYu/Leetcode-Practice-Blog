# Day 36 March eleventh dynamic programming

## [Leetcode 62 Unique Paths](https://leetcode.com/problems/unique-paths/description/)

1. determine dp table and index: dp[i][j] stores how many distinct ways we can reach to `[i - 1][j - 1]` position
2. determine recursion formula: `dp[i][j] = dp[i][j - 1] + dp[i - 1][j]`
3. how to initialize dp table: `dp[i][0]` and `dp[0][j]`. both are 1
4. determine the order of traverse: `dp[i][j]` can be derived from both left and top, level order traversal is enough
5. derive dp by example:


|   | 0 | 1 | 2 | 3  | 4  | 5  | 6  |
| --- | --- | --- | --- | ---- | ---- | ---- | ---- |
| 0 | 1 | 1 | 1 | 1  | 1  | 1  | 1  |
| 1 | 1 | 2 | 3 | 4  | 5  | 6  | 7  |
| 2 | 1 | 3 | 6 | 10 | 15 | 21 | 28 |

```java
public int uniquePaths(int m, int n) {
    int[][] dp = new int[m][n];
    for(int i = 0; i < m; i ++){
        dp[i][0] = 1;
    }
    for(int j = 0; j < n; j ++){
        dp[0][j] = 1;
    }
    for(int i = 1; i < m; i ++){
        for(int j = 1; j < n; j ++){
            dp[i][j] = dp[i - 1][j] + dp[i][j - 1];
        }
    }
    return dp[m - 1][n - 1];
}
```

## [Leetcode 63 Unique Paths II](https://leetcode.com/problems/unique-paths-ii/submissions/913068563/)

1. determine dp table and index: dp[i][j] stores how many distinct ways we can reach to `[i - 1][j - 1]` position, when `obstacleGrid[i][j] == 1`, then `dp[i][j] = 0`
2. determine recursion formula: `dp[i][j] = dp[i][j - 1] + dp[i - 1][j]`
3. how to initialize dp table: `dp[i][0]` and `dp[0][j]`. both are 1, however, if obstacle places in the first row or column, the rest of elements behind obstacle should be 0 at all
4. determine the order of traverse: `dp[i][j]` can be derived from both left and top, level order traversal is enough

```java
public int uniquePathsWithObstacles(int[][] obstacleGrid) {
    int row = obstacleGrid.length;
    int column = obstacleGrid[0].length;
    int[][] dp = new int[row][column];
    if(obstacleGrid[0][0] == 1 || obstacleGrid[row - 1][column - 1] == 1){
        return 0;
    }
    for(int i = 0; i < row && obstacleGrid[i][0] == 0; i ++){
        dp[i][0] = 1;
    }
    for(int j = 0; j < column && obstacleGrid[0][j] == 0; j ++){
        dp[0][j] = 1;
    }
    for(int i = 1; i < row; i ++){
        for(int j = 1; j < column; j ++){
            if(obstacleGrid[i][j] == 1){
                dp[i][j] = 0;
            }
            else{
                dp[i][j] = dp[i - 1][j] + dp[i][j - 1];
            }
        }
    }
    return dp[row - 1][column - 1];
}
```
