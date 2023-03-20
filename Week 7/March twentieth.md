# Day March twentieth Dynamic Programming

## [Leetcode 198 House Robber](https://leetcode.com/problems/house-robber/description/)

### Determine dp table and index

`dp[i]` represents that from house 0 to house i, we can steal at most `dp[i]` money

### Determine recursion formula

When we reach to i room, there are two options, steal or not steal. Plus we need to consider not to alert the police, meanwhile get as much money as we can.

Steal: `dp[i] = dp[i - 2] + nums[i]`, because if we decide to steal i room, we will not steal adjacent room

Not steal: `dp[i] = dp[i - 1]`

### How to initialize dp table

According to recursion formula, we know that we have to initialize `dp[0]` and `dp[1]`

`dp[0] = nums[0]`

`dp[1] = max(nums[0], nums[1])`

### Determine the order of traverse

According to recursion formula, `dp[i]` relies on `dp[i - 1]` and `dp[i - 2]`

### Derive dp by example:


| 0 | 1 | 2  | 3  | 4  |
| --- | --- | ---- | ---- | ---- |
| 2 | 7 | 11 | 11 | 12 |

```java
public int rob(int[] nums) {
    if(nums  == null || nums.length == 0){
        return 0;
    }
    if(nums.length == 1){
        return nums[0];
    }
    int[] dp = new int[nums.length];
    dp[0] = nums[0];
    dp[1] = Math.max(nums[0], nums[1]);
    for(int i = 2; i < nums.length; i ++){
        dp[i] = Math.max(dp[i - 2] + nums[i], dp[i - 1]);
    }
    return dp[nums.length - 1];
}
```

## [Leetcode 213 House Robber II](https://leetcode.com/problems/house-robber-ii/)

This question is very similar with [Leetcode 198 House Robber](https://leetcode.com/problems/house-robber/description/), but it 's a circle rather than a line. So we need to deal with different situations

1. We don't consider the first room and the last one, `1 - nums.length -2`
2. We don't consider the first room `1 - nums.length - 1`
3. We don't consider the last one `0 - nums.length - 2`

Situation 2 and 3 involve situation 1. Therefore we only need to consider situation 2 and 3.

```java
public int rob(int[] nums) {
    if(nums == null || nums.length == 0){
        return 0;
    }
    if(nums.length == 1){
        return nums[0];
    }
    int result1 = robDP(nums, 0, nums.length - 2);
    int result2 = robDP(nums, 1, nums.length - 1);
    return Math.max(result1, result2);
}

public int robDP(int[] nums, int start, int end){
    if(start == end){
        return nums[start];
    }
    int[] dp = new int[nums.length];
    dp[start] = nums[start];
    dp[start + 1] = Math.max(nums[start], nums[start + 1]);
    for(int i = start + 2; i <= end; i ++){
        dp[i] = Math.max(dp[i - 2] + nums[i], dp[i - 1]);
    }
    return dp[end];
}
```

## [Leetcode 337 House Robber III](https://leetcode.com/problems/house-robber-iii/)

Although it's a binary tree structure, this question can still use DP to solve.

<img src="../picture/March%20twentieth/tree1.jpg" width = "400" height = "210" alt="tree1" align=center/>

Two options:

1. When we traverse to node 1, we decide to steal. In this situation, we can't select its siblings. For instance, for ode 3, we can't select node 4 and 5, instead we can select node 1, 3 and 1
2. When we traverse to node 1, we decide not to steal. In this situationm we can select its siblings. For instance, for node 3, we can select node 4 and 5

### Brute force

```java
public int rob(TreeNode root) {
    if(root == null){
        return 0;
    }
    int money = root.val;
    if(root.left != null){
        money += rob(root.left.left) + rob(root.left.right);
    }
    if(root.right != null){
        money += rob(root.right.left) + rob(root.right.right);
    }
    return Math.max(money, rob(root.left) + rob(root.right));
}
```

* Time Complexity: $O(n^2)$
* Space Complexity: $O(logn)$, depends on the depth of recursion

### Dynamic Programming

#### Determine parameters and return value of recursion function, dp table and index

There are two options of one node, steal or not steal. `TreeNode` is the parameter. The return value should be `dp`.

`dp` is an array and its length is 2. `dp[0]` represents the maximum money we can get if we don't steal in this node. `dp[1]` represents the maximum money we can get if we steal in this node

#### Determine the end condition

If we traverse to the `null` node, whether we decide to steal or not, it's all 0, so just return `{0, 0}`

#### Determine logic of single recursion

If we decide to steal the current node, then we can't steal its siblings `value = root.val + left[0] + right[0];`

Otherwise, we can steal the siblings, which one we choose depends on the maximum money we can get `value = max(left[0], left[1]) + max(right[0], right[1]);`

Finally, when we traverse to the last node, the array should be, `{the maximum money we can get if we don't steal the current node, the maximum money we can get if we steal the current node}`

#### Derive dp by example:

<img src="../picture/March%20twentieth/tree2.jpg" width = "400" height = "239" alt="tree2" align=center/>

```java
public int rob(TreeNode root) {
    int[] dp = robRecursion(root);
    return Math.max(dp[0], dp[1]);
}

public int[] robRecursion(TreeNode root){
    int[] dp = new int[2];
    if(root == null){
        return dp;
    }
    int[] left = robRecursion(root.left);
    int[] right = robRecursion(root.right);
    dp[0] = Math.max(left[0], left[1]) + Math.max(right[0], right[1]);
    dp[1] = root.val + left[0] + right[0];
    return dp;
}
```
