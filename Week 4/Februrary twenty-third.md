# Day 22 Februrary twenty-third binary search tree

## [Leetcode 669 Trim a Binary Search Tree](https://leetcode.com/problems/trim-a-binary-search-tree/)

<img src="../picture/Februrary%20twenty-third/trim_node.jpg" width = "400" height = "270" alt="trim_node" align=center/>

At the first glance, we will immediately think about `root.val < low || root.val > high return null`

But it is wrong. For instance as the above picture, when traversing to node 0, 0 is not in the range of [1, 3], then this will cut the entire left sub-tree. But node 0's right sub-tree, node 2 and node 1 are in this range.

1. When value is lower than the range, then check its right sub-tree
2. When value is higher than the range, then check its lef sub-tree
3. traverse left sub-tree and right sub-tree separately

```java
public TreeNode trimBST(TreeNode root, int low, int high) {
    if(root == null){
        return null;
    }
    if(root.val < low){
        return trimBST(root.right, low, high);
    }
    if(root.val > high){
        return trimBST(root.left, low, high);
    }
    root.left = trimBST(root.left, low, high);
    root.right = trimBST(root.right, low, high);
    return root;
}
```

## [Leetcode 108 Convert Sorted Array to Binary Search Tree](https://leetcode.com/problems/convert-sorted-array-to-binary-search-tree/description/)

A sorted array converts to binary search tree

`[-10, -3, 0, 5, 9]` : we take middle, 0 as the root of new tree, so `[-10, -3]` are the left sub-tree, and `[5, 9]` are the right sub-tree.

```java
public TreeNode sortedArrayToBST(int[] nums) {
    return recursion(nums, 0, nums.length - 1);
}
public TreeNode recursion(int[] nums, int low, int high){
    if(low > high){
        return null;
    }
    int middle = (low + high) / 2;
    TreeNode root = new TreeNode(nums[middle]);
    root.left = recursion(nums, low, middle - 1);
    root.right = recursion(nums, middle + 1, high);
    return root;
}
```

## [Leetcode 538 Convert BST to Greater Tree](https://leetcode.com/problems/convert-bst-to-greater-tree/description/)

<img src="../picture/Februrary%20twenty-third/convert_binary_tree.jpg" width = "400" height = "150" alt="convert_binary_tree" align=center/>

This question is very interesting. If you observe carefully, then you may find that it is veryintriguing.

Noticed, we have a **sorted array**, like the above picture. For instance, the accumulate sum of node 3 is 33, `[3, 4, 5, 6, 7, 8]`. Traditionally, it is in-order. But in this question, if we use in-order, we need to traverse all nodes for each time. How about that, we change the sequence of in-order, to `right-root-left`. Then node 3 = `[8, 7, 6, 5, 4, 3]`.

```java
public TreeNode convertBST(TreeNode root) {
    reverseInorder(root);
    return root;
}
public void reverseInorder(TreeNode root){
    if(root == null){
        return;
    }
    reverseInorder(root.right);
    sum += root.val;
    root.val = sum;
    reverseInorder(root.left);
}
```
