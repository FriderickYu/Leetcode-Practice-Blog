# Day 52 March thirtieth Monotonic Stack

## Monotonic Stack

### What is Monotonic Stack

A monotonic stack is a stack whose elements are sorted ascending, like

<img src="../picture/March%20thirtieth/example1.jpg" width = "200" height = "230" alt="example1" align=center/>

### When do we use Monotonic Stack

When we wanna find the position of the first element to the right or left of any element that is larger or smaller than itself, then we use **monotonic stack**. It's kind of *use space to trade with time*. Because we use stack to record the first element that is larger than the current element, so we just need to traverse the entire array for one time.

## [Leetcode 739 Daily Temperatures](https://leetcode.com/problems/daily-temperatures/description/)

`temperatures = [73, 74, 75, 71, 71, 72, 76, 73]`

`result = [1, 1, 4, 2, 1, 1, 0, 0]`

<img src="../picture/March%20thirtieth/step1.jpg" width = "400" height = "150" alt="step1" align=center/>

Step 1: First, push `temperatures[0]` into `stack`

Step 2: Compare `temperatures[0]` with `temperatures[1]`, `temperature[1]` is larger than `temperatures[0]`, so we pop `temperatures[0]`, and push `temperatures[1]`, `result[0]` should be `1 - 0`

<img src="../picture/March%20thirtieth/step2.jpg" width = "400" height = "150" alt="step2" align=center/>

Step 1: Compare `temperature[2]` with `temperatures[1]`, `temperatures[2]` is larger than `temperatures[1]`, so we pop `temperatures[1]`, and push `temperatures[2]`, `result[1]` should be `2 - 1`

<img src="../picture/March%20thirtieth/step3.jpg" width = "400" height = "150" alt="step3" align=center/>

Step 1: Compare `temperatures[3]` with `temperatures[2]`, `temperatures[3]` is smaller than `temperatures[2]`, this is a monotonic stack, so push `temperatures[3]`

Step 2: Compare `temperatures[4]` with `temperatures[3]`, they are equal, so push `temperatures[4]`

<img src="../picture/March%20thirtieth/step4.jpg" width = "400" height = "133" alt="step4" align=center/>

<img src="../picture/March%20thirtieth/step5.jpg" width = "400" height = "162" alt="step5" align=center/>

Step 1: Compare `temperatures[5]` with `temperatures[4]`, `temperatures[5]` is larger than `temperatures[4]`, so we pop `temperatures[4]`, `result[4]` should be `5 - 4`

Step 2: Compare `temperatures[5]` with `temperatures[3]`, `temperatures[5]` is larger than `temperatures[3]`, so we pop `temperatures[3]`, `result[3]` should be `5 - 3`

Step 3: Compare `temperatures[5]` with `temperatures[2]`, `temperatures[5]` is smaller than `temperatures[2]`, so push `temperatures[5]`

<img src="../picture/March%20thirtieth/step6.jpg" width = "400" height = "162" alt="step6" align=center/>

Step 1: Compare `temperatures[6]` with `temperatures[5]`, `temperatures[6]` is larger than `temperaturs[5]`, so we pop `temperatures[5]`, `result[5]` should be `6 - 5`

Step 2: Compare `temperatures[6]` with `temperatures[2]`, `temperatures[6]` is larger than `temperatures[2]`, so we pop `temperatures[2]`, `result[2]` should be `6-2`

Step 3: We push `temperatures[6]`

<img src="../picture/March%20thirtieth/step7.jpg" width = "400" height = "133" alt="step7" align=center/>

Step 1: Compare `temperatures[7]` with `temperatures[6]`, `temperatures[7]` is smaller than `temperature[6]`, so we push `temperatures[7]`

Step 2: There is no element left, so stop the loop; We left `temperatures[6]` and `temperatures[7]` behind, simply because we assign initial value to them

```java
public int[] dailyTemperatures(int[] temperatures) {
    int[] result = new int[temperatures.length];
    Deque<Integer> stack = new LinkedList<>();
    stack.push(0);
    for(int i = 1; i < temperatures.length; i ++){
        if(temperatures[i] <= temperatures[stack.peek()]){
            stack.push(i);
        }
        else{
            while(!stack.isEmpty() && temperatures[i] > temperatures[stack.peek()]){
                result[stack.peek()] = i - stack.peek();
                stack.pop();
            }
            stack.push(i);
        }
    }
    return result; 
}
```

## [Leetcode 496 Next Greater Element I](https://leetcode.com/problems/next-greater-element-i/submissions/924642953/)

```java
public int[] nextGreaterElement(int[] nums1, int[] nums2) {
    Deque<Integer> stack = new LinkedList<>();
    HashMap<Integer, Integer> hashmap = new HashMap<>();
    for(int i = 0; i < nums1.length; i ++){
        hashmap.put(nums1[i], i);
    }
    int[] result = new int[nums1.length];
    Arrays.fill(result, -1);
    stack.push(0);
    for(int i = 1; i < nums2.length; i ++){
        if(nums2[i] <= nums2[stack.peek()]){
            stack.push(i);
        }
        else{
            while(!stack.isEmpty() && nums2[stack.peek()] < nums2[i]){
                if(hashmap.containsKey(nums2[stack.peek()])){
                    Integer index = hashmap.get(nums2[stack.peek()]);
                    result[index] = nums2[i];
                }
                stack.pop();
            }
            stack.push(i);
        }
    }
    return result;
}
```
