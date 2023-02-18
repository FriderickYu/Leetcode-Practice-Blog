# Day 18 Februrary Eighteenth Binary tree

## [Leetcode 513 Find Bottom left Tree Value](https://leetcode.com/problems/find-bottom-left-tree-value/)

Including:

* BFS && queue
* Recursion

### BFS

To find bottom left tree value, is equal to find the first node in the last level. We can use **BFS** to implement it.

```java
public int findBottomLeftValue(TreeNode root) {
    ArrayDeque<TreeNode> queue = new ArrayDeque<>();
    queue.offer(root);
    int result = 0;
    while(!queue.isEmpty()){
        int size = queue.size();
        for(int i = 0; i < size; i ++){
            TreeNode tmpNode = queue.poll();
            if(i == 0){
                result = tmpNode.val;
            }
            if(tmpNode.left != null){
                queue.offer(tmpNode.left);
            }
            if(tmpNode.right != null){
                queue.offer(tmpNode.right);
            }
        }
    }
    return result;
}
```

### Recursion

<img src="../picture/Februrary%20Eighteenth/backtracking_recursion1.jpg" width = "400" height = "250" alt="backtrack and recursion1" align=center/>

At the first glance, our initial thought is to recursively iterate to the left side. However, as the above picture, the result may be 4 rather than 7.

The **precondition** is bottom, and then **leftmost value**.

* find the most depth left leaf
* use **pre-order, in-order and post-order**, to find the leftmost value with the most depth left leaf, as long as left is at the front of right

```java
// record depth
public int depth = -1;
// record left node
public int value = 0;
public int findBottomLeftValue(TreeNode root) {
    value = root.val;
    findLeftValue(root, 0);
    return value;
}
public void findLeftValue(TreeNode root, int deep){
    if(root == null){
        return;
    }
    if(root.left == null && root.right == null){
        if(deep > depth){
            value = root.val;
            depth = deep;
        }
    }
    if(root.left != null){
        findLeftValue(root.left, deep + 1);
    }
    if(root.right != null){
        findLeftValue(root.right, deep + 1);
    }
}
```

## [Leetcode 112 Path Sum](https://leetcode.com/problems/path-sum/)

### Recursion

When recursion needs return?

1. If we want to search the entire binary tree, but do not process the return value, then `void`
2. If we want to search the entire binary tree, and we need to process the return value, then keeps return
3. If we want to search a path which needs to satisfy certain conditions, then we need to keep return

* set count, equal to path sum, when meeting with one node, then minus corresponding value
* if count == 0, and meeting with leaf, then we want it
* if count != 0. and meeting with leaf, then we do not have it

```java
public boolean hasPathSum(TreeNode root, int targetSum) {
    if(root == null){
        return false;
    }
    if(root.left == null && root.right == null){
        return root.val == targetSum;
    }
    if(root.left != null){
        if(hasPathSum(root.left, targetSum - root.val)){
            return true;
        }
    }
    if(root.right != null){
        if(hasPathSum(root.right, targetSum - root.val)){
            return true;
        }
    }
    return false;
}
```

### DFS

```java
public boolean hasPathSum(TreeNode root, int targetSum) {
    if(root == null){
        return false;
    }
    ArrayDeque<TreeNode> stack1 = new ArrayDeque<>();
    ArrayDeque<Integer> stack2 = new ArrayDeque<>();
    stack1.push(root);
    stack2.push(root.val);
    while(!stack1.isEmpty()){
        int size = stack1.size();
        for(int i = 0; i < size; i ++){
            TreeNode tmpNode = stack1.pop();
            int sum = stack2.pop();
            if(tmpNode.left == null && tmpNode.right == null && sum == targetSum){
                return true;
            }
            if(tmpNode.left != null){
                stack1.push(tmpNode.left);
                stack2.push(sum + tmpNode.left.val);
            }
            if(tmpNode.right != null){
                stack1.push(tmpNode.right);
                stack2.push(sum + tmpNode.right.val);
            }
        }
    }
    return false;
}
```

### Similar Questions

[Leetcode 113 Path Sum II](https://leetcode.com/problems/path-sum-ii/)

## [Leetcode 106 Construct Binary Tree from Inorder and Postorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)

We can construct binary tree from:

1. pre-order and in-order
2. in-order and post-order

We can see that, in-order is very important, because from pre-order and post-order, we know that root is start or end. Combining in-order, it is very easy to divide root's left and right.

However, pre-order and post-order can not determine a binary tree.

<img src="../picture/Februrary%20Eighteenth/determine_binary_tree.jpg" width = "400" height = "210" alt="inorder and postorder" align=center/>

1. root is at the end of postorder, so 3 is the root
2. use 3 to divide in-order list, elements are at the front belong to left, and elements are behind belong to right
3. sub-root is at the end of sub-list of postorder, `15, 7, 20`, so 20 is the sub-root

```java
class Solution {
    HashMap<Integer, Integer> map;
    public TreeNode buildTree(int[] inorder, int[] postorder) {
        map = new HashMap<>();
        for(int i = 0; i < inorder.length; i ++){
            map.put(inorder[i], i);
        }
        return findNode(inorder, 0, inorder.length, postorder, 0, postorder.length);
    }
    public TreeNode findNode(int[] inorder, int inBegin, int inEnd, int[] postorder, int postBegin, int postEnd){
        if(inBegin >= inEnd || postBegin >= postEnd){
        return null;
    }
    // find root's index of inorder
    int rootIndex = map.get(postorder[postEnd - 1]);
    // construct node
    TreeNode root = new TreeNode(inorder[rootIndex]);
    int lenOfLeft = rootIndex - inBegin;
    root.left = findNode(inorder, inBegin, rootIndex, postorder, postBegin, postBegin + lenOfLeft);
    root.right = findNode(inorder, rootIndex + 1, inEnd, postorder, postBegin + lenOfLeft, postEnd - 1);
    return root;

}
}
```

### Similar Questions

[Construct Binary Tree from Preorder and Inorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)
