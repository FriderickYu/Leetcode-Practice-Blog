# Day 25 Combination Sum Backtracking

## [Leetcode 39 Combination Sum](https://leetcode.com/problems/combination-sum/description/)

There are several issues that are different from previous questions.

1. The same number may be chosen from `candidates` an unlimited number of times.
2. Pruning is different.

We can sort array first. Therefore the iteration will become more sequential, easier to prune. No need to rush to move steps, because **the same number may be chose from array an unlimited number of times**.

```java
public List<List<Integer>> combinationSum(int[] candidates, int target) {
    List<List<Integer>> result = new ArrayList<>();
    LinkedList<Integer> path = new LinkedList<>();
    Arrays.sort(candidates);
    backtracking(candidates, target, 0, result, path, 0);
    return result;
}
public void backtracking(int[] candidates, int target, int startIndex, List<List<Integer>> result, LinkedList<Integer> path, int sum){
    if(sum > target){
        return;
    }
    if(sum == target){
        result.add(new LinkedList<>(path));
        return;
    }
    for(int i = startIndex; i < candidates.length; i ++){
        if(sum + candidates[i] > target){
            break;
        }
        path.add(candidates[i]);
        sum += candidates[i];
        backtracking(candidates, target, i, result, path, sum);
        path.removeLast();
        sum -= candidates[i];
    }
}
```
