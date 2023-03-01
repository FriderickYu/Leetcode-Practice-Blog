# Day 27 March First backtracking

## [Leetcode 491 Non-decreasing Subsequences](https://leetcode.com/problems/non-decreasing-subsequences/)

<img src="../picture/March%20First/non-decreasing_subsequences.jpg" width = "400" height = "283" alt="non-decreasing_subsequences" align=center/>

There are several issues we need to deal with

1. how to avoid the same element on the same layer: In this question, we cann't sort the entire array and compare adjacent elements. Because once we do this, all sub-lists will become ordered
2. how to arrange a non-decreasing array: when traversing the array, compare the element in the array and the last element of the list, if element in the array is smaller than the element of the list, then this is not the element we want

For instance when we iterate to element `nums[i]`, we need to verify whether we meet the same element before. If yes, ignoring it

We can use `HashMap` to deal with this problem. `key` is the element, and `value` is the frequency. By default, frequency is 0

```java
if(map.getOrDefault(nums[i], 0) >= 1{continue;}
```

```java
public List<List<Integer>> findSubsequences(int[] nums) {
    List<List<Integer>> result = new ArrayList<>();
    LinkedList<Integer> path = new LinkedList<>();
    backtracking(nums, 0, result, path);
    return result;
}
public void backtracking(int[] nums, int startIndex, List<List<Integer>> result, LinkedList<Integer> path){
    if(path.size() > 1){
        result.add(new ArrayList<>(path));
    }
    HashMap<Integer, Integer> map = new HashMap<>();
    for(int i = startIndex; i < nums.length; i ++){
        if(!path.isEmpty() && nums[i] < path.getLast()){
            continue;
        }
        if(map.getOrDefault(nums[i], 0) >= 1){
            continue;
        }
        map.put(nums[i], map.getOrDefault(nums[i], 0) + 1);
        path.add(nums[i]);
        backtracking(nums, i + 1, result, path);
        path.removeLast();
    }
}
```

## [Leetcode 46 Permutations](https://leetcode.com/problems/permutations/description/)

The is a typical permutation problem. Unlike previous questions, such as combination, partitioning, subset. Permutation allows elements to repeat, permutation array is ordered. Like `[1, 2]` and `[2, 1]` are two different set in permutation problem, but they are identical in combination and set

<img src="../picture/March%20First/permutation_tree1.jpg" width = "400" height = "323" alt="permutation_tree1" align=center/>

* The leaf nodes are result we want, when `path.size() == nums.size()`
* Because this is not about combination, partitioning and subset, so we don't need `startIndex`, just traversing from the start in every loop
* If `path` contains one element before, pass

```java
public List<List<Integer>> permute(int[] nums) {
    List<List<Integer>> result = new ArrayList<>();
    LinkedList<Integer> path = new LinkedList<>();
    if(nums.length == 0){
        return result;
    }
    backtracking(nums, result, path);
    return result;
}
public void backtracking(int[] nums, List<List<Integer>> result, LinkedList<Integer> path){
    if(path.size() == nums.length){
        result.add(new ArrayList<>(path));
        return;
    }
    for(int i = 0; i < nums.length; i ++){
        if(path.contains(nums[i])){
            continue;
        }
        path.add(nums[i]);
        backtracking(nums, result, path);
        path.removeLast();
    }
}
```

## [Leetcode 47 Permutations III](https://leetcode.com/problems/permutations-ii/description/)

This question needs to consider de-duplication from the same layer and the same path

For example, like `[1, 1, 2]`

<img src="../picture/March%20First/permutation_tree3.jpg" width = "400" height = "323" alt="permutation_tree3" align=center/>

The first `1` and the second `1`, can repeatly occur in the same path; But they can't appear on the same layer

We create an array named `used` to solve this problem

* if `used[i - 1] == true`, means `nums[i - 1]` is used in the same path, which is alright
* if `used[i - 1] == false`, means `nums[i - 1]` is used in the same layer, not okay
* if `nums[i] == nums[i - 1] && used[i - 1] == false`

```java
boolean[] used;
public List<List<Integer>> permuteUnique(int[] nums) {
    List<List<Integer>> result = new ArrayList<>();
    LinkedList<Integer> path = new LinkedList<>();
    used = new boolean[nums.length];
    Arrays.fill(used, false);
    Arrays.sort(nums);
    backtracking(nums, result, path);
    return result;
}
public void backtracking(int[] nums, List<List<Integer>> result, LinkedList<Integer> path){
    if(path.size() == nums.length){
        result.add(new ArrayList<>(path));
        return;
    }
    for(int i = 0; i < nums.length; i ++){
        if(i > 0 && nums[i] == nums[i - 1] && used[i - 1] == false){
            continue;
        }
        if(used[i] == false){
            used[i] = true;
            path.add(nums[i]);
            backtracking(nums, result, path);
            path.removeLast();
            used[i] = false;
        }
    }
}
```
