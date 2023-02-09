# Day 7 Februrary Seventh Hash Table

## [Leetcode 454 4Sum II](https://leetcode.com/problems/4sum-ii/description/)

Including:

* Hash table
* Loop

There are four arrays, demanding that `nums1[i] + nums2[j] + nums3[k] + nums4[l] == 0`

The initial thought is to set a four level nested loops, to simulate the entire process, but it is $O(n^4)$

So how about this, we set two 2-level nested loops, the first one is to iterate all sums in the first two arrays, and store in `HashMap`. `key` stores `sum` and `value` stores `frequency`.

* Create one `HashMap`, `key` is `sum` and `value` is `frequency` of the first two arrays
* Iterating in the first two arrays, and put sums and corresponding frequency into `HashMap`
* Create a `count` to count frequencies of all `a + b + c + d = 0`
* Iterating in the second two arrays, if `0 - (c + d)` exists in the `HashMap`, then using `count` to count `value`
* Return to `count`

```java
public int fourSumCount(int[] nums1, int[] nums2, int[] nums3, int[] nums4) {
HashMap<Integer, Integer> map = new HashMap<>();
for(int i = 0; i < nums1.length; i ++){
    for(int j = 0; j < nums2.length; j ++){
        if(map.containsKey(nums1[i] + nums2[j])){
            map.put(nums1[i] + nums2[j], map.get(nums1[i] + nums2[j]) + 1);
        }
        else{
            map.put(nums1[i] + nums2[j], 1);
        }
    }
}
int count = 0;
for(int x = 0; x < nums3.length; x ++){
    for(int y = 0; y < nums4.length; y ++){
        if(map.containsKey(0 - (nums3[x] + nums4[y]))){
            count += map.get(0 - (nums3[x] + nums4[y]));
        }
    }
}
return count;
}
```

## [Leetcode 383 Ransom Note](https://leetcode.com/problems/ransom-note/)

Including:

* Array
* Hash table

This question is very similar with [Leetcode 242 Valid Anagram](https://leetcode.com/problems/valid-anagram/description/)

* Store `magazine` into `HashMap`, `key` is `letter`, `value` is `frequency`
* Iterating `ransomNote`
  * if the letter exists in `HashMap`, then `-1`, but if `< 0`, return `false`
  * if the letter does not exist in `HashMap`, then return `false`

```java
HashMap<Character, Integer> map = new HashMap<>();
for(int i = 0; i < magazine.length(); i ++){
    char c = magazine.charAt(i);
    if(map.containsKey(c)){
        map.put(c, map.get(c) + 1);
    }
    else{
        map.put(c, 1);
    }
}
for(int j = 0; j < ransomNote.length(); j ++){
    char c2 = ransomNote.charAt(j);
    if(!map.containsKey(c2)){
        return false;
    }
    else{
        map.put(c2, map.get(c2) - 1);
    }
}
for(Map.Entry<Character, Integer> entry : map.entrySet()){
    if(entry.getValue() < 0){
        return false;
    }
}
return true;
```

## [Leetcode 15 3Sum](https://leetcode.com/problems/3sum/)

Including:

* Hash table
* Double Pointers + Pruning

### Hash Table

Because it is very similar with **Two sum** and **Four Sum**, so `hash table` should be our prior target. Using 2 level nested loop determine `a` and `b`, and `c = 0 - (a + b)`. So `key` is `a + b`, `value` is `list`. But notice, this question needs `de-duplication`, which is very difficult if you use `hash table`

### Double Pointers

Using double pointer is a rational choice, particularly plus **pruning**

<img src="../picture/February%20Seventh/three%20sum.jpg" width = "400" height = "240" alt="three sum" align=center/>

* sort, double pointer method needs a sorted array
* set `left` points `i + 1`, and `right` points `nums.length - 1`
* So `a = nums[i], b = nums[left], c = nums[right]`
* if `nums[i] + nums[left] + nums[right] > 0`, which means sum is larger, then `right --`
* if `nums[i] + nums[left] + nums[right] < 0`, which means sum is smaller, then `left ++`
* When `left >= right`, loop stops
* Now, dealing with de-duplication. De-duplication does not mean one element should appear more than 1 in the list, but means the solution set must not contain duplicate triplets
* `if(i > 0 && nums[i] == nums[i - 1]){continue;}`, de-duplication for `a`
* `while (right > left && nums[right] == nums[right - 1]) right--;` de-duplication for `c`
* `while (right > left && nums[left] == nums[left + 1]) left++;` de-duplication for `b`

```java
kpublic List<List<Integer>> threeSum(int[] nums) {
List<List<Integer>> result = new ArrayList<>();
Arrays.sort(nums);
for(int i = 0; i < nums.length; i ++){
    if(nums[i] > 0){
        return result;
    }
    if(i > 0 && nums[i] == nums[i - 1]){
        continue;
    }
    int left =  i + 1;
    int right = nums.length - 1;
    while(right > left){
        int sum = nums[i] + nums[left] + nums[right];
        if(sum > 0){
            right --;
        }
        else if(sum < 0){
            left ++;
        }
        else{
            result.add(Arrays.asList(nums[i], nums[left], nums[right]));
            while (right > left && nums[right] == nums[right - 1]) right--;
            while (right > left && nums[left] == nums[left + 1]) left++;
  
            right--; 
            left++;
        }
    }
}
return result;
}
```

## [Leetcode 18 4Sum](https://leetcode.com/problems/4sum/)

Including:

* Double Pointers

This question is very similar with ***3Sum***, except one thing.

* `a, b, c, d` are distinct
* `target` may be positve, 0, and negative

Especially in `pruning`, the condition is quite different

```java
List<List<Integer>> result = new ArrayList<>();
Arrays.sort(nums);
for(int i = 0; i < nums.length; i ++){
    // difference 1, because target could be negative and 0
    if(nums[i] > 0 && nums[i] > target){
        return result;
    }
    if(i > 0 && nums[i] == nums[i - 1]){
        continue;
    }
    // four numbers
    for(int j = i + 1; j < nums.length; j ++){
        if(j > i + 1 && nums[j] == nums[j - 1]){
            continue;
        }
        int left = j + 1;
        int right = nums.length - 1;
        while(right > left){
            long sum = (long)nums[i] + nums[j] + nums[left] + nums[right];
            if(sum > target){
                right --;
            }
            else if(sum < target){
                left ++;
            }
            else{
                result.add(Arrays.asList(nums[i], nums[j], nums[left], nums[right]));
                while(right > left && nums[right] == nums[right - 1]){
                    right --;
                }
                while(right > left && nums[left] == nums[left + 1]){
                    left ++;
                }
                right --;
                left ++;
            }
        }
    }
}
return result;
```
