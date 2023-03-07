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
