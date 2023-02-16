# Day 15 Februrary Sixteenth Binary tree

## [Leetcode 104 Maximum Depth of binary tree](https://leetcode.com/problems/maximum-depth-of-binary-tree/)

Including:

* binary tree
* Level-order traversal

Depth and height, mean various in the binary tree

<img src="../picture/Februrary%20Sixteenth/maximum_depth.jpg" width = "400" height = "380" alt="maximum depth" align=center/>

Depth: from the root to this node, the max number of nodes

Height: from this node to its children nodes, the max number of nodes

Generally, to calculate depth, we use pre-order; To calculate height, we use post-order

### Recursion

```java
public int maxDepth(TreeNode root) {
    if(root == null){
        return 0;
    }
    int leftDepth = maxDepth(root.left);
    int rightDepth = maxDepth(root.right);
    return Math.max(leftDepth, rightDepth) + 1;
}
```

### Level order traversal

```java
public int maxDepth(TreeNode root) {
    if(root == null){
        return 0;
    }
    ArrayDeque<TreeNode> queue = new ArrayDeque<>();
    queue.offer(root);
    int depth = 0;
    while(!queue.isEmpty()){
        int size = queue.size();
        depth ++;
        for(int i = 0; i < size; i ++){
            TreeNode node = queue.poll();
            if(node.left != null){
                queue.offer(node.left);
            }
            if(node.right != null){
                queue.offer(node.right);
            }
        }
    }
    return depth;
}
```

## [Leetcode 111 Minimum Depth of Binary Tree](https://leetcode.com/problems/minimum-depth-of-binary-tree/)

```java
public int minDepth(TreeNode root) {
    if (root == null) {
        return 0;
    }
    int leftDepth = minDepth(root.left);
    int rightDepth = minDepth(root.right);
    if (root.left == null) {
        return rightDepth + 1;
    }
    if (root.right == null) {
        return leftDepth + 1;
    }
    return Math.min(leftDepth, rightDepth) + 1;
}
```
