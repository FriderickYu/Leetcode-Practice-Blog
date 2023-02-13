# Day 13 Februrary Thirteenth Stack & Queue

## [Leetcode 239 Sliding Window Maximum](https://leetcode.com/problems/sliding-window-maximum/)

Including；

* Brute-force and simulation
* Sliding window
* Queue

## Brute force

<img src="../picture/Februrary%20Thirteenth/brute%20force.jpg" width = "300" height = "120" alt="brute force" align=center/>

This method is basically simulating the entire process

* Define the range
* Find the maximum value in this range
* Add results into list

```java
public int[] maxSlidingWindow(int[] nums, int k) {
    ArrayList<Integer> list = new ArrayList<>();
    for(int i = 0; i < i + k; i ++){
        if(i + k <= nums.length){
            int maxValue = Solution.findMax(nums, i, i + k);
            list.add(maxValue);
        }
    }
    return list.stream().mapToInt(x -> x).toArray();
}
public static int findMax(int[] arr, int left, int right){
    int max = Integer.MIN_VALUE;
    for(int i = left; i < right; i ++){
        if(arr[i] > max){
            max = arr[i];
        }
    }
    return max;
}
```

However, this method exceeds the time limit

## Sliding Windows and Queue

Regardless of different methods, this question mainly focuses on two questions:

* How to move forward -> sliding windows
* How to find the max -> ?

`Queue` is the suitable data structure.

<img src="../picture/Februrary%20Thirteenth/queue1.jpg" width = "300" height = "153" alt="queue1" align=center/>

So this is the first part, if we want to move `1~3`, then `pop` 1 and insert -3. The maximum of `0~2` is 3.

However, this is not such function in `Queue`. So we need to design by ourselves.

### Monotonic Queue

* `pop` : if the element which is removed by windows is equal to `peek`, then `pop`
* `push` : if the value we push is larger than the last value of `queue`, remove this last value. Until the value we push is equal or smaller than the last value of `queue`

Therefore, `peek` is exactly the maximum value in each part

<img src="../picture/Februrary%20Thirteenth/process1.jpg" width = "300" height = "263" alt="process1" align=center/>

1. k = 3, start with 0~2, first `push` 1 into `Queue`
2. next is 3, 1 < 3, so remove 1 and `push` 3
3. `push` -1, 3 is the peek, so it's max
4. move to 1~3, `push` -3, 3 is the max
5. move to 2~4, `pop` 3 and `push` 5, because -1 and -3 smaller than 5, so remove -1 and -3, 5 is the max
6. ...

```java
class MyQueue{
    ArrayDeque<Integer> queue = new ArrayDeque<>();
    void poll(int val){
        if(!queue.isEmpty() && val == queue.peek()){
            queue.poll();
        }
    }
    void add(int val){
        while(!queue.isEmpty() && val > queue.getLast()){
            queue.removeLast();
        }
        queue.add(val);
    }
    int peek(){
        return queue.peek();
    }
}
public int[] maxSlidingWindow(int[] nums, int k) {
    if(nums.length == 1){
        return nums;
    }
    ArrayList<Integer> list = new ArrayList<>();
    int num = 0;
    MyQueue queue = new MyQueue();
    for(int i = 0; i < k; i ++){
        queue.add(nums[i]);
    }
    list.add(queue.peek());
    for(int i = k; i < nums.length; i ++){
        queue.poll(nums[i - k]);
        queue.add(nums[i]);
        list.add(queue.peek());
    }
    return list.stream().mapToInt(x -> x).toArray();
}
```

## [Leetcode 347 Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/description/)

Including:

* HashMap
* Sort
* PriorityQueue(Heap)

`HashMap` is random sorted, therefore we choose `TreeMap` instead

* Convert `treemap` to `list`
* Implement `Comparator` interface descendingly
* Use `Entry` to iterate list, and use subList to slice

```java
public int[] topKFrequent(int[] nums, int k) {
    TreeMap<Integer, Integer> map = new TreeMap<>();

    for(int i = 0; i < nums.length; i ++){
        if(!map.containsKey(nums[i])){
            map.put(nums[i], 1);
        }
        else{
            map.put(nums[i], map.get(nums[i]) + 1);
        }
    }
    ArrayList<Map.Entry<Integer, Integer>> list = new ArrayList<>(map.entrySet());
    Collections.sort(list, new Comparator<Map.Entry<Integer, Integer>>() {
        @Override
        public int compare(Map.Entry<Integer, Integer> o1, Map.Entry<Integer, Integer> o2) {
            return o2.getValue().compareTo(o1.getValue());
        }
    });
    ArrayList<Integer> arrayList = new ArrayList<>();
    for(Map.Entry<Integer, Integer> mapping : list.subList(0, k)){
        arrayList.add(mapping.getKey());
    }
    return arrayList.stream().mapToInt(x -> x).toArray();
}
```
