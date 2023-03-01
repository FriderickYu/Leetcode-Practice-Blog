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
