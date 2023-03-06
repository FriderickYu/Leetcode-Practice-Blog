# Day 31 March Sixth Greedy

## [Leetcode 1005 Maximize Sum of Array After K Negations](https://leetcode.com/problems/maximize-sum-of-array-after-k-negations/description/)

* local optimum: always finds the minimum value each time, and replace it with negation
* global optimum: Sum of the entire array is maximum

```java
public int largestSumAfterKNegations(int[] nums, int k) {
   for(int count = 0; count < k; count ++){
       Arrays.sort(nums);
       nums[0] *= -1;
   }
   int sum = 0;
   for(int value : nums){
       sum += value;
   }
   return sum;
}
```

## [Leetcode 134 Gas Station](https://leetcode.com/problems/gas-station/description/)
