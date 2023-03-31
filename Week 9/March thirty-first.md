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
