# Day 14 Februrary Fourteenth Tree

In daily life, two kinds of binary tree are normal:

* full binary tree
* complete binary tree

## Full Binary Tree

If degree of nodes either is 0 or 2, and those degree are 0, *on the same layer*. Then this binary tree is a **full binary tree**

<img src="../picture/Februrary%20Fourteenth/complete_binary_tree.png" width = "300" height = "238" alt="complete binary tree" align=center/>

This tree is a typical *full binary tree*. The depth is $k$. So a typical full binary tree has $2^k-1$ nodes.

## Complete Binary Tree

In a complete binary tree, the number of nodes at each level reaches the maximum$(2^{k-1})$, except for the bottom, which is underfilled. And nodes at the bottom level are concentrated at **the leftmost positions**.

<img src="../picture/Februrary%20Fourteenth/complete_binary_tree1.jpg" width = "400" height = "176" alt="complete binary tree1" align=center/>

## Binary Search Tree

binary search tree is a ordered tree:

* If root has left child, then all nodes on the left sub-tree are **smaller** than root.
* If root has right child, then all nodes on the right sub-tree are **bigger** than root.
* Root's left and right sub-trees are also binary search tree.

## Traversal

* DFS: Pre, in, and posr refer to the position of the root
  * Pre-order: root, left, right
  * In-order: left, root, right
  * Post-order: left, right, root
* BFS
  * Level-order

<img src="../picture/Februrary%20Fourteenth/DFS.jpg" width = "400" height = "204" alt="DFS" align=center/>

1. Pre-order: 5412678
2. In-order: 1425768
3. Post-order: 1247865

## Construction of binary tree

```java
class TreeNode{
    int val;
    TreeNode left;
    TreeNode right;

    public TreeNode() {}
    public TreeNode(int val) {
        this.val = val;
    }
    public TreeNode(int val, TreeNode left, TreeNode right) {
        this.val = val;
        this.left = left;
        this.right = right;
    }
}
```

## Recursive traversal of binary tree

There are three crucial elements in recursion:

1. parament and return value
2. Exit
3. Logic of the single recursion

Take pre-order as an example:

* parament and return value: `preorder(root, val);`
* Exit: `if(root == null) return;`
* Logic: `result.add(root); preorder(root.left, result); preorder(root.right, result);`

### [Leetcode 144 Binary Tree Preorder Traversal](https://leetcode.com/problems/binary-tree-preorder-traversal/)

```java
public List<Integer> preorderTraversal(TreeNode root) {
    ArrayList<Integer> list = new ArrayList<>();
    preorder(root, list);
    return list;
}
public void preorder(TreeNode root, List<Integer> list){
    if(root == null){
        return;
    }
    list.add(root.val);
    preorder(root.left, list);
    preorder(root.right, list);
}
```

### [Leetcode 94 Binary Tree Inorder Traversal](https://leetcode.com/problems/binary-tree-inorder-traversal/)

```java
public List<Integer> inorderTraversal(TreeNode root) {
    ArrayList<Integer> list = new ArrayList<>();
    inorder(root, list);
    return list;
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

### [Leetcode 145 Binary Tree Postorder](https://leetcode.com/problems/binary-tree-postorder-traversal/description/)

```java
public List<Integer> postorderTraversal(TreeNode root) {
    ArrayList<Integer> list = new ArrayList<>();
    postorder(root, list);
    return list;
}
public void postorder(TreeNode root, List<Integer> list){
    if(root == null){
        return;
    }
    postorder(root.left, list);
    postorder(root.right, list);
    list.add(root.val);
}
```

## Non-recursive traversal of binary tree

In DFS, we always use `stack` to implement it.

### Pre-order

<img src="../picture/Februrary%20Fourteenth/dfs_preorder1.jpg" width = "400" height = "180" alt="DFS preorder" align=center/>

1. 5 is the root, then push into stack
2. 4 and 6 are chilrdren of 5, but **push 6 first**, and then push 4
3. So, 4 will pop first, 546

```java
public List<Integer> preorderTraversal(TreeNode root) {
    ArrayList<Integer> list = new ArrayList<>();
    if(root == null){
        return list;
    }
    ArrayDeque<TreeNode> stack = new ArrayDeque<>();
    stack.push(root);
    while(!stack.isEmpty()){
        TreeNode node = stack.pop();
        list.add(node.val);
        if(node.right != null){
            stack.push(node.right);
        }
        if(node.left != null){
            stack.push(node.left);
        }
    }
    return list;
}
```

### In-order

<img src="../picture/Februrary%20Fourteenth/dfs_inorder1.jpg" width = "400" height = "227" alt="DFS inorder" align=center/>

* push root into stack
* find whether root has left child, if yes, then push into stack
* if not, then push right child into stack

```java
public List<Integer> inorderTraversal(TreeNode root) {
    ArrayList<Integer> list = new ArrayList<>();
    ArrayDeque<TreeNode> stack = new ArrayDeque<>();
    if(root == null){
        return list;
    }
    TreeNode current = root;
    while(current != null || !stack.isEmpty()){
        if(current != null){
            stack.push(current);
            current = current.left;
        }
        else{
            current = stack.pop();
            list.add(current.val);
            current = current.right;
        }
    }
    return list;
}
```

### Post-order

So in the pre-order, it is root-left-right. Because we use stack, so the sequence should be: `pop(root), pop(right), pop(left)`

But in the post-order, it is left-right-root, reverse it become to root-right-left, so we just need to modify  pre-order code -> `pop(root), pop(left), pop(right)`, and `reverse`

```java
public List<Integer> postorderTraversal(TreeNode root) {
    ArrayList<Integer> list = new ArrayList<>();
    if(root == null){
        return list;
    }
    ArrayDeque<TreeNode> stack = new ArrayDeque<>();
    stack.push(root);
    while(!stack.isEmpty()){
        TreeNode node = stack.pop();
        list.add(node.val);
        if(node.left != null){
            stack.push(node.left);
        }
        if(node.right != null){
            stack.push(node.right);
        }
    }
    Collections.reverse(list);
    return list;
}
```

