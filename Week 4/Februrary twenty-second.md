# Day 21 Februrary twenty-second binary search tree

## [Leetcode 235 Lowest Common Ancestor of a Binary Search Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/description/)

This question is very similar with [Leetcode 236 Lowest Common Ancestor of a Binary Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/).
Except in this time, it is binary search tree. Therefore, node p and q's ancestor must be in the range of `[p, q]`. So iterating from the top to end,
when we meet a node that is in the range of `[p, q]`, then that is it.

```java
public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
    if(root == null){
        return null;
    }
    if(root.val < p. val && root.val < q.val){
        return lowestCommonAncestor(root.right, p, q);
    }
    if(root.val > p.val && root.val > q.val){
        return lowestCommonAncestor(root.left, p, q);
    }
    return root;
}
```

## [Leetcode 701 Insert into a Binary Search Tree](https://leetcode.com/problems/insert-into-a-binary-search-tree/description/)

```java
public TreeNode insertIntoBST(TreeNode root, int val) {
    if(root == null){
        return new TreeNode(val);
    }
    if(root.val < val){
        root.right = insertIntoBST(root.right, val);
    }
    else if(root.val > val){
        root.left = insertIntoBST(root.left, val);
    }
    return root;
}
```

## [Leetcode 450 Delete Node in a BST](https://leetcode.com/problems/delete-node-in-a-bst/)

Delete a node in BST is rather tough, we need to think carefully.

1. we do not find the node we want to delete, just return null
2. we do find the node we want to delete
   1. if the node, which is exactly we want to delete, its left and right sub-trees are empty, then just directly delete the node, and return null
   2. if left sub-tree of node is null, and right is not null, deleting node and return `node.right`
   3. if right sub-tree of node is null, and left is not null, deleting node and return `node.left`
   4. if both left and right sub-trees are not null, then putting left node of the node, on the right sub-tree's leftmost child. At last, deleting the node, and return `node.right`

<img src="../picture/Februrary%20twenty-second/delete_node.jpg" width = "400" height = "150" alt="delete_node" align=center/>

```java
public TreeNode deleteNode(TreeNode root, int key) {
  root = delete(root, key);
  return root;
}
public TreeNode delete(TreeNode root, int key){
  if(root == null){
      return null;
  }
  if(root.val > key){
      root.left = delete(root.left, key);
  }
  else if(root.val < key){
      root.right = delete(root.right, key);
  }
  // we find the node we wanna delete
  else{
      if(root.left == null){
          return root.right;
      }
      if(root.right == null){
          return root.left;
      }
      // if left and right are not null, search the leftmost child of the right
      // return to right
      TreeNode tmpNode = root.right;
      while(tmpNode.left != null){
          tmpNode = tmpNode.left;
      }
      root.val = tmpNode.val;
      root.right = delete(root.right, tmpNode.val);
  }
  return root;
}
```
