# Day 20 Februrary twenty-first Binary search tree

## [Leetcode 530 Minimum Absolute Difference in BST](https://leetcode.com/problems/minimum-absolute-difference-in-bst/description/)

<img src="../picture/Februrary%20twenty-first/bst.jpg" width = "292" height = "301" alt="bst" align=center/>

A binary search tree, like the above shows. If we use in-order to traverse, then it will be `1, 2, 3, 4, 6`, completely ordered.

At last, compare every two nodes' substraction, because it is ordered, then no need to consider `abs`.

```java
public int getMinimumDifference(TreeNode root) {
    List<Integer> list = new ArrayList<>();
    inorder(root, list);
    int min = Integer.MAX_VALUE;
    for(int i = 0; i < list.size() - 1; i ++){
        for(int j = i + 1; j < list.size(); j ++){
            if(list.get(j) - list.get(i) < min){
                min = list.get(j) - list.get(i);
            }
        }
    }
    return min;
}
public void inorder(TreeNode root, List<Integer> list){
    if(root == null){
        return;
    }
    inorder(root.left, list);
    list.add(root.val);
    inorder(root.right, list);
}
```

## [Leetcode 501 Find mode in binary search tree](https://leetcode.com/problems/find-mode-in-binary-search-tree/description/)

<img src="../picture/Februrary%20twenty-first/traverse_reverse.jpg" width = "400" height = "180" alt="traverse_reverse" align=center/>

In previous questions, we always traverse binary search, from top to end. But this time, we want to search reversely, from end to top. Therefore, we need backtracking, `post-order` is a great option to backtrack.

<img src="../picture/Februrary%20twenty-first/traverse_reverse2.jpg" width = "400" height = "220" alt="traverse_reverse2" align=center/>

This is the most common example, 7 and 4 are two nodes we want to find their ancestor. Node 2, we want that 7 is in its left sub-tree, 4 is in its right sub-tree. Then node 2 is the ancestor we want to find.

So, if we iterate to p, then just return p, if it is q, then return q. Because we use **post-order** to iterate. Hence, if either p, or q is not null, which means the root of p and q, is the lowest common ancestor.

<img src="../picture/Februrary%20twenty-first/traverse_reverse3.jpg" width = "400" height = "208" alt="traverse_reverse3" align=center/>

This is another situation, actually it is the same with the previous one. If we iterate to p, then return p. This includes that p is q's ancestor.

```java
public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
    if(root == null){
        return null;
    }
    if(root == p || root == q){
        return root;
    }
    TreeNode left = lowestCommonAncestor(root.left, p, q);
    TreeNode right = lowestCommonAncestor(root.right, p, q);
    if(left == null && right == null){
        return null;
    }
    else if(left == null && right != null){
        return right;
    }
    else if(left != null && right == null){
        return left;
    }
    else{
        return root;
    }
}
```
