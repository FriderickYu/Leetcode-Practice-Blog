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

## [Leetcode 40 Combination Sum II](https://leetcode.com/problems/combination-sum-ii/description/)

* Each number is `candidates` may only be used **once** in the combination
* The solution set must not contain duplicate combinations

We should notice that, there are duplicate numbers in the array, therefore
if we don't de-duplicate the array, set is very possible contain duplication

<img src="../picture/Februrary%20twenty-seventh/de-duplication.jpg" width = "400" height = "200" alt="de-duplication" align=center/>

Therefore, what we need to de-duplicate is the "used" elements on the same tree level, elements on the same branch are all elements of the same combination and do not need to be de-duplicated

```java
public List<List<Integer>> combinationSum2(int[] candidates, int target) {
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
    }
    for(int i = startIndex; i < candidates.length; i ++){
        if(sum + candidates[i] > target){
            break;
        }
        if(i > startIndex && candidates[i] == candidates[i - 1]){
            continue;
        }
        path.add(candidates[i]);
        sum += candidates[i];
        backtracking(candidates, target, i + 1, result, path, sum);
        path.removeLast();
        sum -= candidates[i];
    }
}
```
