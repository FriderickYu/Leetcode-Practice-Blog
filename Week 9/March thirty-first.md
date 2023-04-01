# Day 53 March thirty-first Monotonic Stack

## [Leetcode 503 Next Greater Element II](https://leetcode.com/problems/next-greater-element-ii/description/)

So this question is very similar with [Leetcode 739 Daily Temperatures](https://leetcode.com/problems/daily-temperatures/description/), except this is a circular array.

It's very simple to implement circular array, we just need to make the original array to double, like

`nums = [1, 2, 1]`

`new_nums = [1, 2, 1, 1, 2, 1]`

And in the end, we need to reduce our `result` to half, that is enough

```java
public int[] nextGreaterElements(int[] nums) {
    Deque<Integer> stack = new LinkedList<>();
    if(nums == null || nums.length <= 1){
        return new int[]{-1};
    }
    int[] new_nums = new int[2 * nums.length];
    for(int i = 0; i < nums.length; i ++){
        new_nums[i] = nums[i];
        new_nums[i + nums.length] = nums[i];
    }
    System.out.println(Arrays.toString(new_nums));
    int[] result = new int[new_nums.length];
    Arrays.fill(result, -1);
    stack.push(0);
    for(int i = 1; i < new_nums.length; i ++){
        if(new_nums[i] <= new_nums[stack.peek()]){
            stack.push(i);
        }
        else{
            while(!stack.isEmpty() && new_nums[i] > new_nums[stack.peek()]){
                result[stack.peek()] = new_nums[i];
                stack.pop();
            }
            stack.push(i);
        }
    }
    return Arrays.copyOfRange(result, 0, (result.length / 2));
}
```

## [Leetcode 42 Trapping Rain Water](https://leetcode.com/problems/trapping-rain-water/)

Example:

`height = {0, 1, 0, 2, 1, 0, 1, 3, 2, 1, 2, 1}`

`sum = 6`

### Brute Force & Double Pointer

<img src="../picture/March%20thirty-first/trap1.jpg" width = "400" height = "222" alt="trap1" align=center/>

There are two directions to think about this problem, from row, or from column. In this problem, we use from column(as yellow arrow shows).

We take column 4 as an example to illustrate our idea. To trap the water, we need to examine its right and left

1. To the left, the largest is column 3, it's 2
2. To the right, the largest is column 7, it's 3
3. The volume depends on the minimum, so it relies on column 3, then use the height of column 3 minus column 4's height, `2 - 1`

So in summary, certain column's height should be `min(rHeight, lHeight) - height`

Step 1: We don't trap water in the first and last columns

```java
if(i == 0 || i == height.length - 1){
    continue;
}
```

Next we will find the largest height of column, from left and right directions, use `height[i]` as the benchmark

```java
for(int r = i + 1; r <= height.length - 1; r ++){
    if(height[r] > rHeight){
        rHeight = height[r];
    }
}
for(int l = i - 1; l >= 0; l --){
    if(height[l] > lHeight){
        lHeight = height[l];
    }
}
```

After we find it, we can calculate the height of current column

```java
int h = Math.min(lHeight, rHeight) - height[i];
if(h > 0){
    sum += h;
}
```

The drawback it very obvious, time complexity is $O(n^2)$

```java
public static int trap(int[] height) {
    int sum = 0;
    for(int i = 0; i < height.length; i ++){
        if(i == 0 || i == height.length - 1){
            continue;
        }
        int lHeight = height[i];
        int rHeight = height[i];
        for(int r = i + 1; r <= height.length - 1; r ++){
            if(height[r] > rHeight){
                rHeight = height[r];
            }
        }
        for(int l = i - 1; l >= 0; l --){
            if(height[l] > lHeight){
                lHeight = height[l];
            }
        }
        int h = Math.min(lHeight, rHeight) - height[i];
        if(h > 0){
            sum += h;
        }
    }
    return sum;
}
```

### Double Pointer, Optimization

**Brute force(double pointer)** is very easy to understand, but it still needs to be implemented.

When we search to the highest of right and left, take column 4 as an example:

* Left range: `[0, 4)`
* Right range: `(4, 11]`

But in column 5, it's

* Left range: `[0, 5)`
* Right range: `(5, 11]`

We can see that in column 4 and 5, the search range has overlapping area, *duplication*. So the optimization focuses on how to reduce such duplication.

* The highest height of the left, `maxLeft[i] = max(height[i], maxLeft[i - 1])`
* The highest height of the right, `maxRight[i] = max(height[i], maxRight[i + 1])`

<img src="../picture/March%20thirty-first/trap2.jpg" width = "400" height = "132" alt="trap2" align=center/>

```java
public int trap(int[] height) {
    int[] maxLeft = new int[height.length];
    int[] maxRight = new int[height.length];
    maxLeft[0] = height[0];
    for(int i = 1; i < height.length; i ++){
        maxLeft[i] = Math.max(height[i], maxLeft[i - 1]);
    }
    maxRight[height.length - 1] = height[height.length - 1];
    for(int i = height.length - 2; i >= 0; i --){
        maxRight[i] = Math.max(height[i], maxRight[i + 1]);
    }
    int sum = 0;
    for(int i = 0; i < height.length; i ++){
        int h = Math.min(maxLeft[i], maxRight[i]) - height[i];
        if(h > 0){
            sum += h;
        }
    }
    return sum;
}
```

### Monotonic Stack
