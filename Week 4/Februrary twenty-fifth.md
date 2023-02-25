# Day 24 Februrary twenty-fifth backtracking

## [Leetcode 216 Combination Sum III](https://leetcode.com/problems/combination-sum-iii/description/)

This question is very similar with [Leetcode 77 Combinations](https://leetcode.com/problems/combinations/).

<img src="../picture/Februrary%20twenty-fifth/combination_sum.jpg" width = "400" height = "279" alt="combination_sum" align=center/>

The first step is `end condition`, after fully analyzing the detail. We know that the number of left nodes must be k, and the sum should be n. So end condition should be `path.size() && sum == n`

For each path, as long as the iteration is moving pass via the path, add it to the list, then don't forget accumulate to add value into `sum`, then recursion, backtracking

Yes, this question can be improved based on pruning

1. same as [Leetcode 77 Combinations](https://leetcode.com/problems/combinations/), the range of i
2. if the sum is larger than n, then it's no need to process further, like `[2,4], [2,5]`

```java
public List<List<Integer>> combinationSum3(int k, int n) {
    List<List<Integer>> result = new ArrayList<>();
    LinkedList<Integer> path = new LinkedList<>();
    backtracking(k, n, 1, result, path, 0);
    return result;
}
public void backtracking(int k, int n, int startIndex, List<List<Integer>> result, LinkedList<Integer> path, int sum){
    if(sum > n){
        return;
    }
    if(path.size() == k && sum == n){
        result.add(new LinkedList<>(path));
        return;
    }
    for(int i = startIndex; i <= 9 - (k - path.size()) + 1; i ++){
        
        path.add(i);
        sum += i;
        backtracking(k, n, i + 1, result, path, sum);
        path.removeLast();
        sum -= i;
    }
}
```