# Shell Sort

Shell Sort is an improvement on insertion sort

* Insertion sort is very efficient if a list is largely sorted. So, if a list is completely sorted, then it is just iterating for one time,$O(n)$. However, comparing to bubble sort and selection sort, these still need compare
* Insertion sort only moves one number for a time

## Implementation and Idea

Shell sort uses *Shell increment* to divide the entire list, Shell increment is $\frac {n}{2^k}, k = 1, 2, 3...$, if there are 10 elements, then increment should be `{5, 2, 1}`

* Set shell increment, the end is when shell increment is 1
* In each increment, use insertion sort

```java
public static int[] shellSort(int[] arr){
    if(arr.length < 2){
        return arr;
    }
    for(int gap = arr.length / 2; gap > 0; gap /= 2){
        for(int start = 0; start < gap; start ++){
            for(int i = start + gap; i < arr.length; i += gap){
                for(int j = i; j > 0; j -= gap){
                    if(arr[j] < arr[j - 1]){
                        test.swap(arr, j, j - 1);
                    }
                    else{
                        break;
                    }
                }
            }
        }
    }
    return arr;
}
```

### Why time complexity of shell sort is smaller than $O(n^2)$

So, an order means values is from smallest to the largest. If a pair number, the number before is greater than the number after, like `i < j, arr[i] > arr[j]`, that is called `inverse order`. In an array, the total sum of `inverse order`, we call it `inverse order number`. The process of sorting is basically keep reducing the `inverse order number` to 0.

For instance, bubble and insertion, each time we just swap the adjacent number. So each time we just -1 of `inverse order number` each time. But in selection sort, because we swap not adjacent number, which means sometimes we can reduce multiple `inverse order number` each time.

## Analysis

Time Complexity

* Average: $O(nlogn)$
* Worst: $O(n)$

Space Complexity: $O(1)$

Stable: ***unstable***

## Optimization

Hibbard Increment: $2^k - 1$ worst time complexity: $O(n^{\frac {3}{2}})$

Knuth Increment: $\frac {3^k-1}{2}$ worst time complexity: $O(n^{\frac {3}{2}})$

Sedgewich Increment: $4^k+3 \times 2^{k-1}$ worst time complexity: $O(n^{\frac {4}{3}})$
