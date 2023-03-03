# Day 29 March Third Greedy

The essence of greedy algorithm, is to choose the local optimum at each stage in order to achieve the global optimum

1. Dividing problem into several small pieces
2. Finding an appropriate solution
3. Finding the local optimum solution
4. Accumulating the local optimal to gloabl optimal

## [Leetcode 45 Assign Cookies](https://leetcode.com/problems/assign-cookies/description/)

A large cookie 5, could satisfy a kid with small appetite 1, also could satisfy a kid with larger appetite 5. To achieve the local optimum, we should satisfy larger appetite first

* Local optimum: Larger cookie gives to larger appetite first
* Gloabl optimum: Feed as many children as possible

sort two array first, and iterating the children array, from the end to start. If cookie is larger than children, then count + 1

<img src="../picture/March%20Third/appetite.jpg" width = "400" height = "133" alt="appetite" align=center/>

```java
public int findContentChildren(int[] g, int[] s) {
    Arrays.sort(g);
    Arrays.sort(s);
    int count = 0;
    int start = s.length - 1;
    for(int i = g.length - 1; i >= 0; i --){
        if(start >= 0 && g[i] <= s[start]){
            start --;
            count ++;
        }
    }
    return count;
}
```

## [Leetcode 376 Wiggle Subsequence](https://leetcode.com/problems/wiggle-subsequence/description/)

Wiggle subsequence needs turbulence in the array

<img src="../picture/March%20Third/wiggle_subsequence.jpg" width = "400" height = "200" alt="wiggle_subsequence" align=center/>

The rational choice is to cancel elements on the monotonous slope, except two endpoints

```java
public int wiggleMaxLength(int[] nums) {
    if(nums.length <= 1){
        return nums.length;
    }
    int curDiff = 0;
    int preDiff = 0;
    int count = 1;
    if(nums.length == 2){
        if(nums[0] == nums[1]){
            return 1;
        }
        else{
            return 2;
        }
    }
    for(int i = 1; i < nums.length; i ++){
        curDiff = nums[i] - nums[i - 1];
        if((curDiff > 0 && preDiff <= 0) || (curDiff < 0 && preDiff >= 0)){
            count ++;
            preDiff = curDiff;
        }
    }
    return count;
}
```
## [Leetcode 53 Maximum Subarray](https://leetcode.com/problems/maximum-subarray/description/)

* Local optimum: when the consistent sum is smaller than 0, then quit, and continue to calculate the sum from the next element.
* Global optimum: iterating the rest of array

```java
public int maxSubArray(int[] nums) {
    if(nums.length == 1){
        return nums[0];
    }
    int sum = Integer.MIN_VALUE;
    int count = 0;
    for(int i = 0; i < nums.length; i ++){
        count += nums[i];
        sum = Math.max(sum, count);
        if(count < 0){
            count = 0;
        }
    }
    return sum;
}
```
