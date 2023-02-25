# Day 23 Februrary twenty-fourth backtracking

## Backtracking

Actually, backtracking is a **by-product** of recursion. As long as there is recursion, there will be backtracking.

Backtracking is not a very efficient algorithm, because the essence of backtracking is **exhaustive**, exhausting all possibilities and then selecting the answer we want.

### Categories

* Combination: like find the set of `k` numbers in `N` numbers according to certain rules. Ex: in `[1, 2, 3, 4]`, `[1, 2]`, `[1, 3]`, `[1, 4]`, `[2, 3]`, `[2, 4]`, `[3, 4]`
* Cutting: how many ways a string can be cut according to certain ways
* Subset: how many eligible subsets are there in a set of N numbers
* Permutation: how many ways we can arrange N numbers, according to certain ways
* The board: N queens, Sudoku

### Process

Backtracking can be abstracted to tree structure, and because basically it is about to find subset in a large set, so:

1. The size of set constructs the width of tree
2. The depth of recursion constructs the depth of tree
3. Recursion needs end condition, so the depth of tree must be limited

Taking an example to illustrate our idea, we have a set `[1, 2, 3, 4]`, we want to find subsets, which consists of 2 numbers of the set

<img src="../picture/Februrary%20twenty-fourth/backtracking.jpg" width = "400" height = "279" alt="backtracking" align=center/>

The above picture demonstrates the abstract tree structure of backtracking

Generally speaking, when we move to the leaf node, then we find one of the solution

<img src="../picture/Februrary%20twenty-fourth/leaf_node.jpg" width = "400" height = "282" alt="leaf_node" align=center/>

```java
if(end conditions)
{
    store the result(leaf node)
    return;
}
```

<img src="../picture/Februrary%20twenty-fourth/backtracking_withdraw.jpg" width = "400" height = "178" alt="backtracking_withdraw" align=center/>

Watch this backtracking & recursion process

* iterate the set, `[1, 2, 3, 4]`
* process each node, for instance, node 1 has three children, node 2, 3, 4, `{1}`
* recursion all children of node 1
  * add node 2 into list, so `{1, 2}`
  * because node 2 is a leaf node, so backtracking to node 1 and remove node 2 from the list, `{1}`
  * add node 3 into list, so `{1, 3}`

```java
for(iterate the set to generate nodes){
    add value of path
    // recursion
    backtracking(paths, list);
    backtracking to the previous node, withdraw the process
}
```

So, here is the code template for **backtracking**

```java
void backtracking(parameters){
	if(end conditions){
		store the result // leaf node
		return;
	}
	for(iterate the set to generate nodes){
		add value of path
		// recursion
		backtracking(paths, list);
		backtracking to the previous node, withdraw the process
	}
}
```

## [Leetcode 77 Combinations](https://leetcode.com/problems/combinations/)

<img src="../picture/Februrary%20twenty-fourth/combination.jpg" width = "400" height = "211" alt="combination" align=center/>

Surely we can solve this problem by nested loop. But if n=100, and k=50, then time complexity if too huge. But backtracking is really doing this work.

1. select one element from the original set
2. select another element from the set

In this question, n is the width of tree, k is the depth of tree

<img src="../picture/Februrary%20twenty-fourth/combination_process.jpg" width = "400" height = "241" alt="combination_process" align=center/>

* end condition: as the above picture shows, when we meet left nodes, then add them to the list; The criteria to verify left nodes is that, when elements which node contains is equal to k
* Next, we will generate nodes in layer 2(exclude root and left nodes), we use `startIndex` to record the first index of each sub-root
  * add element of paths into collection
  * recursion layer3(left nodes)
  * backtrack

```java
public List<List<Integer>> combine(int n, int k) {
    List<List<Integer>> result = new ArrayList<>();
    LinkedList<Integer> path = new LinkedList<>();
    backtracking(n, k, 1, result, path);
    return result;
}
public void backtracking(int n, int k, int startIndex, List<List<Integer>> result, LinkedList<Integer> path){
    // end condition
    if(path.size() == k){
        result.add(new LinkedList<>(path));
        return;
    }
    for(int i = startIndex; i <= n; i ++){
        path.add(i);
        backtracking(n, k, i + 1, result, path);
        path.removeLast();
    }
}
```

### Improvement Pruning

<img src="../picture/Februrary%20twenty-fourth/pruning.jpg" width = "400" height = "327" alt="pruning" align=center/>

If n=4, and k=4, just like this picture,we know that most paths and nodes are useless. So if the number of `startIndex` and elements after it is smaller than k, then no need to search further

So we need to narrow the range down `for(iterate set to generate nodes)`

* the number of elements which we already add(ex, path between layer1 and layer2): `path.size()`
* we still need: `k - path.size()`
* traverse the set n from this starting position at most: `n - (k - path.size()) + 1`

For instance, in layer 2, the upper range will be `4 - (4 - 1) + 1 = 2`. In layer 3, the upper range will be `4 - (4 - 2) + 1 = 3`, etc.

```java
public List<List<Integer>> combine(int n, int k) {
    List<List<Integer>> result = new ArrayList<>();
    LinkedList<Integer> path = new LinkedList<>();
    backtracking(n, k, 1, result, path);
    return result;
}
public void backtracking(int n, int k, int startIndex, List<List<Integer>> result, LinkedList<Integer> path){
    // end condition
    if(path.size() == k){
        result.add(new LinkedList<>(path));
        return;
    }
    for(int i = startIndex; i <= n - (k - path.size()) + 1; i ++){
        path.add(i);
        backtracking(n, k, i + 1, result, path);
        path.removeLast();
    }
}
```
