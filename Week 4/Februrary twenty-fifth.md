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

## [Leetcode 17 Letter Combinations of a Phone Number](https://leetcode.com/problems/letter-combinations-of-a-phone-number/description/)

There are two problems that we need to deal with:

1. how to map numbers to letters
2. how to design recursion and backtracking processes

### How to map numbers to letters

```java
String[] map = 
{"", // 0
"", // 1
"abc", // 2
"def", // 3
"ghi", // 4
"jkl", // 5
"mno", // 6
"pqrs", // 7
"tuv", // 8
"wxyz" // 9
};
```

### How to design recursion and backtracking processes

<img src="../picture/Februrary%20twenty-fifth/phone_numbers.jpg" width = "400" height = "190" alt="phone_numbers" align=center/>

1. set `index` to record which number currently we point, also `index` indicates the depth of tree. Like `23`, so it only needs to iterate two layers. The end condition is `if(index == digits.size())`
2. convert number to letter, `String str = map[digits.charAt(index) - '0']`
3. add value of every path into string
4. recursion, and backtracking

```java
StringBuilder sb = new StringBuilder();
public List<String> letterCombinations(String digits) {
    List<String> result = new ArrayList<>();
    if(digits == null || digits.length() == 0){
        return result;
    }
    String[] map = {"", "", "abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"};
    backtracking(digits, map, result, 0);
    return result;
}
public void backtracking(String digits, String[] map, List<String> result, int index){
    if(index == digits.length()){
        result.add(sb.toString());
        return;
    }
    String str = map[digits.charAt(index) - '0'];
    for(int i = 0; i < str.length(); i ++){
        sb.append(str.charAt(i));
        backtracking(digits, map, result, index + 1);
        sb.deleteCharAt(sb.length() - 1);
    }
}
```
