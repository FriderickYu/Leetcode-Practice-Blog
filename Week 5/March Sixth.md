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

## [Leetcode 135 Candy](https://leetcode.com/problems/candy/)

1. Each child must have at least one candy
2. Children with a higher rating get more candies than their neighbors, including right and left

So, we can consider this problem from two sides

* From start to end: `if(ratings[i] > ratings[i - 1]){candy[i] = candy[i - 1] + 1;}`

* From end to start: `if(ratings[i] > ratings[i + 1]{candy[i] = Math.max(candy[i], candy[i + 1] + 1);}`

ratings: 1 2 2 5 4 3 2

Candy1: 1 2 1 2 1 1 1

Candy2: 1 1 1 4 3 2 1

Candy 1 2(max) 1 4 3 2 1

```java
public int candy(int[] ratings) {
    int[] candy = new int[ratings.length];
    candy[0] = 1;
    for(int i = 1; i < ratings.length; i ++){
        if(ratings[i] > ratings[i - 1]){
            candy[i] = candy[i - 1] + 1;
        }
        else{
            candy[i] = 1;
        }
    }
    for(int j = ratings.length - 2; j >= 0; j --){
        if(ratings[j] > ratings[j + 1]){
            candy[j] = Math.max(candy[j], candy[j + 1] + 1);
        }
    }
    int sum = 0;
    for(int num : candy){
        sum += num;
    }
    return sum;
}
```