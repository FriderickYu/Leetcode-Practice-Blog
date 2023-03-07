# Day 32 March Seventh Greedy

## [Leetcode 860 Lemonade Change](https://leetcode.com/problems/lemonade-change/)

There are three different situations:

1. bill is 5, taking it
2. bill is 10, return to one 5, and get this 10
3. bill is 20
   1. consuming one 10 and one 5
   2. if 10 or 5 is not enough, consuming three 5

```java
public boolean lemonadeChange(int[] bills) {
    int five = 0;
    int ten = 0;
    for(int i = 0; i < bills.length; i ++){
        if(bills[i] == 5){
            five ++;
        }
        else if(bills[i] == 10){
            five --;
            ten ++;
        }
        else if(bills[i] == 20){
            if(ten > 0){
                five --;
                ten --;
            }
            else{
                five -= 3;
            }
        }
        if(five < 0 || ten < 0){
            return false;
        }
    }
    return true;
}
```

## [Leetcode 406 Queue Reconstruction by Height](https://leetcode.com/problems/queue-reconstruction-by-height/)

```java
public int[][] reconstructQueue(int[][] people) {
  Arrays.sort(people, (a, b) -> {
      if (a[0] == b[0]) return a[1] - b[1];
      return b[0] - a[0];
  });

  LinkedList<int[]> que = new LinkedList<>();

  for (int[] p : people) {
      que.add(p[1],p);
  }

  return que.toArray(new int[people.length][]);
}
```

## [Leetcode 452 Minimum Number of Arrows to Burst Balloons](https://leetcode.com/problems/minimum-number-of-arrows-to-burst-balloons/description/)

```java
class Solution {
   public int findMinArrowShots(int[][] points) {
      // 根据气球直径的开始坐标从小到大排序
      // 使用Integer内置比较方法，不会溢出
      Arrays.sort(points, (a, b) -> Integer.compare(a[0], b[0]));

      int count = 1;  // points 不为空至少需要一支箭
      for (int i = 1; i < points.length; i++) {
         if (points[i][0] > points[i - 1][1]) {  // 气球i和气球i-1不挨着，注意这里不是>=
            count++; // 需要一支箭
         } else {  // 气球i和气球i-1挨着
            points[i][1] = Math.min(points[i][1], points[i - 1][1]); // 更新重叠气球最小右边界
         }
      }
      return count;
   }
}
```