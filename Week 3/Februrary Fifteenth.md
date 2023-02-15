# Day 15 Februrary Fifteenth Binary tree

## [Leetcode 102 Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal/)

BFS needs `queue` to implement.

<img src="../picture/Februrary%20Fifteenth/bfs.jpg" width = "400" height = "319" alt="bfs" align=center/>

As the above picture shows:

1. `push` root 6 into `queue`
2. root 6 has two children, 4 and 7, then `pop` 6 and `push` 4 and 7
3. `pop` 4, 4 has two children, 1 and 3, `push` them into `queue`
4. `pop` 7, 7 has two children, 5 and 8

```java
public List<List<Integer>> levelOrder(TreeNode root) {
    List<List<Integer>> result = new ArrayList<List<Integer>>();
    bfs(root, result);
    return result;
}

public void bfs(TreeNode node, List<List<Integer>> result){
    if(node == null){
        return;
    }
    ArrayDeque<TreeNode> queue = new ArrayDeque<>();
    queue.offer(node);
    while(!queue.isEmpty()){
        ArrayList<Integer> list = new ArrayList<>();
        int len = queue.size();
        while(len > 0){
            TreeNode tmpNode = queue.poll();
            list.add(tmpNode.val);
            if(tmpNode.left != null){
                queue.offer(tmpNode.left);
            }
            if(tmpNode.right != null){
                queue.offer(tmpNode.right);
            }
            len --;
        }
        result.add(list);
    }
}
```

### Similar Questions

[Leetcode 107 Binary Tree Level order Traversal II](https://leetcode.com/problems/binary-tree-level-order-traversal-ii/description/)

[Leetcode 199 Binary Tree Right Side View](https://leetcode.com/problems/binary-tree-right-side-view/description/)

[Leetcode 637 Average of Levels in Binary Tree](https://leetcode.com/problems/average-of-levels-in-binary-tree/description/)

[Leetcode 429 N-ary Tree Level Order Traversal](https://leetcode.com/problems/n-ary-tree-level-order-traversal/description/)

## [Leetcode 226 Invert Binary Tree](https://leetcode.com/problems/invert-binary-tree/)

*Too easy*

```java
public TreeNode invertTree(TreeNode root) {
    if(root == null){
        return null;
    }
    swap(root);
    invertTree(root.left);
    invertTree(root.right);
  
    return root;
}
public void swap(TreeNode root){
    TreeNode tmpNode = root.left;
    root.left = root.right;
    root.right = tmpNode;
}
```

## [Leetcode 101 Symmetric Tree](https://leetcode.com/problems/symmetric-tree/description/)

```java
public boolean isSymmetric(TreeNode root) {
    return compare(root.left, root.right);
}
public boolean compare(TreeNode left, TreeNode right){
    if(left == null && right != null){
        return false;
    }
    if(left != null && right == null){
        return false;
    }
    if(left == null && right == null){
        return true;
    }
    if(left.val != right.val){
        return false;
    }
    boolean compareOutSide = compare(left.left, right.right);
    boolean compareInside = compare(left.right, right.left);
    return compareOutSide && compareInside;
}
```
