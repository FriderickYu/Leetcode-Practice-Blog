# Day 28 March Second backtracking

## [Leetcode 332 Reconstruct Itinerary](https://leetcode.com/problems/reconstruct-itinerary/)

## [Leetcode 51 N-Queens](https://leetcode.com/problems/n-queens/)

In chessboard, there are constraint conditions of queens:

1. two queens can't be on the same row
2. two queens can't be on the same row
3. two queens can't be on the same diagonal line

<img src="../picture/March%20Second/3queens.jpg" width = "400" height = "438" alt="3queens" align=center/>

This picture illustrates the process of three queens, horizontally it iterates **each column**, vertically it iterates **each row**. Eventually we collect each leaf nodes. (three queens doesn't have solution).

```java
if(row == n){
    result.add(chessboard);
    return;
}
```

Let's begin with horizontal direction, it starts with column, so

```java
for(int column = 0; column < n; column ++){
    if(isValid(row, column, n, chessboard)){
        chessboard[row][column] = 'Q';
        backtracking(n, row + 1, chessboard);
        chessboard[row][column] = '.';
    }
}
```

Finally, we should analyze whether this chessboard is legal according to the constraint conditions.

```java
List<List<String>> result = new ArrayList<>();
public List<List<String>> solveNQueens(int n) {
    char[][] chessboard = new char[n][n];
    for(char[] c : chessboard){
        Arrays.fill(c, '.');
    }
    backtracking(n, 0, chessboard);
    return result;
}
public void backtracking(int n, int row, char[][] chessboard){
    if(row == n){
        result.add(joint(chessboard));
        return;
    }
    for(int column = 0; column < n; column ++){
        if(isValid(row, column, n, chessboard)){
            chessboard[row][column] = 'Q';
            backtracking(n, row + 1, chessboard);
            chessboard[row][column] = '.';
        }
    }
}
public List<String> joint(char[][] chessboard){
    List<String> list = new ArrayList<>();
    for(char[] c : chessboard){
        list.add(String.copyValueOf(c));
    }
    return list;
}
public boolean isValid(int row, int column, int n, char[][] chessboard){
    // 检查列
    for(int i = 0; i < row; i ++){
        if(chessboard[i][column] == 'Q'){
            return false;
        }
    }
    // 检查45度对角线
    for(int i = row - 1, j = column - 1; i >= 0 && j >= 0; i --, j --){
        if(chessboard[i][j] == 'Q'){
            return false;
        }
    }
    // 检查135度对角线
    for(int i = row - 1, j = column + 1; i >= 0 && j <= n - 1; i --, j ++){
        if(chessboard[i][j] == 'Q'){
            return false;
        }
    }
    return true;
}
```

## [Leetcode 37 Sudoku Solver](https://leetcode.com/problems/sudoku-solver/)
