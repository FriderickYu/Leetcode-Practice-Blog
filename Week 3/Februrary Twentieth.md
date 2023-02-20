# Day 19 Februrary Twentieth binary tree

## [Leetcode 654 Maximum Binary Tree](https://leetcode.com/problems/maximum-binary-tree/)

<img src="../picture/Februrary%20Twentieth/654.jpg" width = "400" height = "250" alt="654" align=center/>

This question is like the combination of **sliding window** and **findMax**.

1. use sliding window to control the range
2. find the max value in the range, set it to root
3. if there are elements at the left to the root, then means this root has left children
4. if there are elements at the right to the root, then means this root has right children

```java
public TreeNode constructMaximumBinaryTree(int[] nums) {
    return findMaximumRecursion(nums, 0, nums.length);
}
public TreeNode findMaximumRecursion(int[] nums, int low, int high){
    if(low >= high){
        return null;
    }
    int maxIndex = low;
    int maxValue = nums[maxIndex];
    for(int i = low + 1; i < high; i ++){
        if(nums[i] > maxValue){
            maxIndex = i;
            maxValue = nums[i];
        }
    }
    TreeNode root = new TreeNode(maxValue);
    root.left = findMaximumRecursion(nums, low, maxIndex);
    root.right = findMaximumRecursion(nums, maxIndex + 1, high);
    return root;
}
```

## [Leetcode 617 Merge Two Binary Trees](https://leetcode.com/problems/merge-two-binary-trees/description/)

### Recursion

```java
public TreeNode mergeTrees(TreeNode root1, TreeNode root2) {
    if(root1 == null){
        return root2;
    }
    if(root2 == null){
        return root1;
    }
    root1.val += root2.val;
    root1.left = mergeTrees(root1.left, root2.left);
    root1.right = mergeTrees(root1.right, root2.right);
    return root1;
}
```

## DFS

```java
public TreeNode mergeTrees(TreeNode root1, TreeNode root2) {
    if(root1 == null){
        return root2;
    }
    if(root2 == null){
        return root1;
    }
    ArrayDeque<TreeNode> stack = new ArrayDeque<>();
    stack.push(root1);
    stack.push(root2);
    while(!stack.isEmpty()){
        TreeNode node2 = stack.pop();
        TreeNode node1 = stack.pop();
        node1.val += node2.val;
        if(node2.left != null && node1.left != null){
            stack.push(root1.left);
            stack.push(root2.left);
        }
        else{
            if(node1.left == null){
                node1.left = node2.left;
            }
        }
        if(node2.right != null && node1.right != null){
            stack.push(root1.right);
            stack.push(root2.right);
        }
        else{
            if(node1.right == null){
                node1.right = node2.right;
            }
        }
    }
    return root1;
}
```

## [Leetcode 700 Search in a Binary Search Tree](https://leetcode.com/problems/search-in-a-binary-search-tree/description/)

Binary Search tree, is a search sorted tree

* if root's left child is not null, then all nodes in left child are smaller than root
* if root's right child is not null, then all nodes in right child are bigger than root
* root's left and right sub-tree are also binary search tree

```java
public TreeNode searchBST(TreeNode root, int val) {
    if(root == null || root.val == val){
        return root;
    }
    if(val < root.val){
        return searchBST(root.left, val);
    }
    else{
        return searchBST(root.right, val);
    }
}
```

## [Leetcode 98 Validate Binary Search Tree](https://leetcode.com/problems/validate-binary-search-tree/description/)

```java
public boolean isValidBST(TreeNode root) {
    ArrayList<Integer> array = new ArrayList<>();
    inorder(root, array);
    for(int i = 0; i < array.size() - 1; i ++){
        if(array.get(i) >= array.get(i + 1)){
            return false;
        }
    }
    return true;
}
public void inorder(TreeNode root, ArrayList<Integer> array){
  
    if(root == null){
        return;
    }
    inorder(root.left, array);
    array.add(root.val);
    inorder(root.right, array);
}
```
