# Day 26 Februrary twenty-eighth backtracking

## [Leetcode 93 Restore IP Addresses](https://leetcode.com/problems/restore-ip-addresses/description/)

The question, and [Leetcode 131 Palindrome Partitioning](https://leetcode.com/problems/palindrome-partitioning/description/) belong to a same problem, **partitioning**. In partitioning problem, `startIndex` is always used to indicate where we slice.

<img src="../picture/Februrary%20twenty-eighth/partition1.jpg" width = "400" height = "140" alt="de-partition1" align=center/>

* how to partition: `[startIndex, i]`
* what is the end condition: when dots are three, partition whole string. If the last sub-string is legal, then adding it into list
* how to determine a sub-string is legal: if it begins with 0, then it can't contain other numbers; it must be smaller than 9 and larger than 0; it must be smaller than 255

```java
class Solution {
    public List<String> restoreIpAddresses(String s) {
        List<String> result = new ArrayList<>();
        if(s.length() > 12){
            return result;
        }
        backtracking(s, 0, 0, result);
        return result;
    }
    public void backtracking(String s, int startIndex, int dot_number, List<String> result){
        if(dot_number == 3){
            if(isValid(s, startIndex, s.length() - 1)){
                result.add(s);
            }
            return;
        }
        for(int i = startIndex; i < s.length(); i ++){
            if(isValid(s, startIndex, i)){
                s = s.substring(0, i + 1) + "." + s.substring(i + 1);
                dot_number ++;
                backtracking(s, i + 2, dot_number, result);
                s = s.substring(0, i + 1) + s.substring(i + 2);
                dot_number --;
            }
            else{
                break;
            }
        }
    }
    public boolean isValid(String s, int start, int end){
        if(start > end){
            return false;
        }
        if(s.charAt(start) == '0' && start != end){
            return false;
        }
        int number = 0;
        for(int i = start; i <= end; i ++){
            if(s.charAt(i) > '9' || s.charAt(i) < '0'){
                return false;
            }
            number = number * 10 + (s.charAt(i) - '0');
            if(number > 255){
                return false;
            }
        }
        return true;
    }
}
```

## [Leetcode 78 Subsets](https://leetcode.com/problems/subsets/description/)

This is a typical **subset** problem. Unlike problems we previously deal with, those are combination and partitioning problems, which we also abstract to binary tree structure, and answers are always leaf nodes.

In subset problem, it's quite different.

<img src="../picture/Februrary%20twenty-eighth/subset.jpg" width = "400" height = "270" alt="subset" align=center/>

We need to find every nodes in the tree, which means traverse the entire tree.

```java
public List<List<Integer>> subsets(int[] nums) {
    List<List<Integer>> result = new ArrayList<>();
    LinkedList<Integer> path = new LinkedList<>();
    backtracking(nums, 0, result, path);
    return result;
}
public void backtracking(int[] nums, int startIndex, List<List<Integer>> result, LinkedList<Integer> path){
    result.add(new LinkedList<>(path));
    if(startIndex >= nums.length){
        return;
    }
    for(int i = startIndex; i < nums.length; i ++){
        path.add(nums[i]);
        backtracking(nums, i + 1, result, path);
        path.removeLast();
    }
}
```

## [Leetcode 90 Subsets II](https://leetcode.com/problems/subsets-ii/description/)

This question is similar with the mixture of [Leetcode 78 Subsets](https://leetcode.com/problems/subsets/description/) and [Leetcode 40 Combination Sum II](https://leetcode.com/problems/combination-sum-ii/description/)

```java
public List<List<Integer>> subsetsWithDup(int[] nums) {
    List<List<Integer>> result = new ArrayList<>();
    LinkedList<Integer> path = new LinkedList<>();
    Arrays.sort(nums);
    backtracking(nums, 0, result, path);
    return result;
}
public void backtracking(int[] nums, int startIndex, List<List<Integer>> result, LinkedList<Integer> path){
    result.add(new LinkedList<>(path));
    if(startIndex >= nums.length){
        return;
    }
    for(int i = startIndex; i < nums.length; i ++){
        if(i > startIndex && nums[i] == nums[i - 1]){
            continue;
        }
        path.add(nums[i]);
        backtracking(nums, i + 1, result, path);
        path.removeLast();
    }
}
```
