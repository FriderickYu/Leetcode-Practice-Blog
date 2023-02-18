# Day 17 Februrary Seventeenth binary tree

## [Leetcode 110 Balanced Binary Tree](https://leetcode.com/problems/balanced-binary-tree/)

Including:

* Binary Tree
* recursion
* AVL

### What is Balanced Binary Tree

Binary search has many advantages, but obviously search time depends on the depth. If depth is higher enough then binary search tree will lost balance.

In balanced binary tree, we have a thing called **balanced factor**. It means the depth of left sub-tree minus the depth of right sub-tree. Balanced factor of all nodes in the binary search tree could only be -1, 0, 1.

<img src="../picture/Februrary%20Seventeenth/avl1.jpg" width = "400" height = "340" alt="avl1" align=center/>

This is not a balanced tree, because several nodes' balanaced factors are not equal to -1, 0, 1.

```java
public boolean isBalanced(TreeNode root) {
    return getHeight(root) == -1 ? false : true;
}
public int getHeight(TreeNode root){
    if(root == null){
        return 0;
    }
    int leftHeight = getHeight(root.left);
    // if left sub-tree is not AVL
    if(leftHeight == -1){
        return -1;
    }
    int rightHeight = getHeight(root.right);
    // if right sub-tree is not AVL
    if(rightHeight == -1){
        return -1;
    }
    if(Math.abs(leftHeight - rightHeight) > 1){
        return -1;
    }
    return Math.max(leftHeight, rightHeight) + 1;
}
```

## [Leetcode 257 Binary Tree Paths](https://leetcode.com/problems/binary-tree-paths/)

Including:

* binary tree
* recursion
* backtracking

<img src="../picture/Februrary%20Seventeenth/backtrack.jpg" width = "400" height = "270" alt="backtrack" align=center/>

Backtracking is a rational choice. The mission basically is to find the leaf node. Here is the process:

1. 1->2
2. 1->2->4, 4 is the leaf
3. backtrack to 2
4. 1->2->5, 5 is the leaf
5. backtrack to 2, and all 2's children have been founded
6. backtrack to 1
7. 1->3, 3 is the leaf
8. backtrack to 1, and all 1's children have been founded

```java
public List<String> binaryTreePaths(TreeNode root) {
    // store the final result
    List<String> result = new ArrayList<>();
    if(root == null){
        return result;
    }
    List<Integer> paths = new ArrayList<>();
    traversal(root, paths, result);
    return result;
}
public void traversal(TreeNode root, List<Integer> paths, List<String> result){
    paths.add(root.val);
    // recursion exit
    if(root.left == null && root.right == null){
        StringBuilder sb = new StringBuilder();
        for(int i = 0; i < paths.size() - 1; i ++){
            sb.append(paths.get(i)).append("->");
        }
        sb.append(paths.get(paths.size() - 1));
        result.add(sb.toString());
    }
    // backtracking
    if(root.left != null){
        traversal(root.left, paths, result);
        paths.remove(paths.size() - 1);
    }
    if(root.right != null){
        traversal(root.right, paths, result);
        paths.remove(paths.size() - 1);
    }
}
```

## [Leetcode 404 Sum of Left Leaves](hhttps://leetcode.com/problems/sum-of-left-leaves/)

what is the left leaves. In conclusion, if node A's left child exists, and A's left children, both left and right are `null`. Hence, node's left child is the left leaf

`if(node.left != null && node.left.left == null && node.left.right == null)`

```java
public int sumOfLeftLeaves(TreeNode root) {
    if(root == null){
        return 0;
    }
    if(root.left == null && root.right == null){
        return 0;
    }
    int leftValue = sumOfLeftLeaves(root.left);
    int rightValue = sumOfLeftLeaves(root.right);
    int value = 0;
    if(root.left != null && root.left.left == null && root.left.right == null){
        value = root.left.val;
    }
    return value + leftValue + rightValue;
}
```
