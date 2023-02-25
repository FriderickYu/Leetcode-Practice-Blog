# Merge Sort

Merge sort is a typical *divide and conquer* algorithm, basically using **recursion or iteration** to divide the entire list, until the lenght of list is 1, and incorporate all sub-list altogether.

## Implementation and Idea

### Non-in situ incorporation(with extra data structure)

#### top-down

What is top-down? The idea of top-down, combining with non-in situ, is the most easiest way to think in this question. Given a list, you divide it into several sub-lists whose length is 1. And then merge these sub-lists. Hence, divide is top-down, and merge is down-top.

This is the process of divide.

<img src="../picture/Common%20Sorting%20Algorithms/merge_sort/top_down1.jpg" width = "400" height = "313" alt="top_down1" align=center/>

This process is relatively easy, we can borrow the idea of *binary search*,
using two pointers, `low` indicates the start, and `high` indicates the end.
`middle` is `(low + high) / 2`. Dividing the list into 2 pieces according to `middle`.

```java
if(low < high){
    int middle = low + (high - low) / 2;
    mergeSortRecursion(arr, tmpArr, low, middle);
    mergeSortRecursion(arr, tmpArr, middle + 1, high);
}
```

And this is the process of merge.

<img src="../picture/Common%20Sorting%20Algorithms/merge_sort/top_down2.jpg" width = "400" height = "459" alt="top_down2" align=center/>

What is really pain in the ass, is the process of merge.

<img src="../picture/Common%20Sorting%20Algorithms/merge_sort/merge1.jpg" width = "400" height = "247" alt="merge1" align=center/>

Taking the above picture as an example, we assume that it moves to the last step, two ordered sub-list need to merge into one single ordered list.

1. We need an extra data structure, an array, named `tmpArr`, its length is equal to the original array, `arr`
2. Because we use `middle` to divide the list, therefore we use `lh` to point `l`, `rh` to point the next element of middle, `head` to point the initial element of `tmpArr`
3. Now we know the range of loop. `lc <= middle && rh <= high`. In this range, we will compare each element from two lists, list A and B. If element in listA is smaller than listB's, then put it into `tmpArr`, `lh` moves to the next, and `head` moves to the next simultaneously. Otherwise, put element of listB into `tmpArr` `rh` moves to the next. and `head` moves to the next simultaneously.
4. After the loop, if listA or listB still has some elements left, then we need to whether which one left something:
   1. if it is listA, then we just put rest of elements of listA into the end of `tmpArr`
   2. if it is listB, then we just put rest of elements of listB into the end of `tmpArr`
5. Copying every elements of `tmpArr` into the original array

```java
public static void main(String[] args) {
  int[] array = {4, 6, 2, 1, 7, 9, 5, 8, 3};
  System.out.println(Arrays.toString(mergeSort(array)));
}
public static int[] mergeSort(int[] nums){
  if(nums.length < 2){
      return nums;
  }
  int[] tmpArr = new int[nums.length];
  mergeSortRecursion(nums, tmpArr, 0, nums.length - 1);
  return nums;
}
public static void mergeSortRecursion(int[] nums, int[] tmpArr, int low, int high){
  if(low < high){
      int middle = low + (high - low) / 2;
      mergeSortRecursion(nums, tmpArr, low, middle);
      mergeSortRecursion(nums, tmpArr, middle + 1, high);
      merge(nums, tmpArr, low, middle, high);
  }
}
public static void merge(int[] nums, int[] tmpArr, int low, int middle, int high){
  int lh = low;
  int rh = middle + 1;
  int head = low;
  while(lh <= middle && rh <= high){
      if(nums[lh] <= nums[rh]){
          tmpArr[head] = nums[lh];
          lh ++;
      }
      else{
          tmpArr[head] = nums[rh];
          rh ++;
      }
      head ++;
  }
  while(lh <= middle){
      tmpArr[head] = nums[lh];
      head ++;
      lh ++;
  }
  while(rh <= high){
      tmpArr[head] = nums[rh];
      head ++;
      rh ++;
  }
  /* Java, copying one array to another
  *  https://www.geeksforgeeks.org/array-copy-in-java/
  * */
  for(; low <= high; low ++){
      nums[low] = tmpArr[low];
  }
}
```

#### down-top

### In situ incorporation

#### top-down

#### down-top

## Analysis

Stable: When we merge sub-list, it is `if(arr[lh] <= arr[rh])`. So it guarantees that there are same elements in the list, the left-handed element is always placed on the left-handed side, so **stable**

### Non-in situ incorporation(with extra data structure)

#### top-down

Time Complexity: Times of dividing array into half is $logn$, means the recursion is $logn$. So after dividing, we need to compare elements from each side, and putting elements in `tmpArr`, `if(nums[lh] <= nums[rh]){tmpArr[head] = nums[lh];}`. Time complexity of comparison and assigning value are both $O(n)$. Overall, it's $O(nlogn)$

Space Complexity: The depth of recursion is $log(n)$, but we use an extra array, so $O(n)$

## Optimization
