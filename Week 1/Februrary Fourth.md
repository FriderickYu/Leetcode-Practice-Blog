# Day 4 Februrary Fourth

## Leetcode 24 Swap Nodes in Pairs

[Video Analysis](https://www.bilibili.com/video/BV1YT411g7br/?spm_id_from=333.788&vd_source=e8f9779956746463d5471d7c18ccae92)

Including:

* Linked List
* Double Pointers
* Recursion
* Dummy Head

### Double Pointers

First, this question is quite suitable for using `dummy head`. Because after the swap, `head`node will change. So a stable`dummy head`is helpful

The following graphs show the process

set `dummy head`point to the real head `A`, and set `curr`point to `dummy head`

<img src="/picture/Februrary%20Fourth/double%20pointer%20process%201.jpg" width = "400" height = "160" alt="process 1" align=center/>

Then making movement, `curr.next = curr.next.next`, but this movement may lost `node A`, so we have to use `temp`to store `node A`

<img src="../picture/Februrary%20Fourth/double%20pointer%20process%202.jpg" width = "400" height = "160" alt="process 2" align=center/>

Now, we have to let `node B` point to `node A`, `curr.next.next = temp`, but this movement may lost `node C`, so we have to use `temp1` to store
`node C`. Finally, let `node A` point to `node C`, `temp.next = temp1`

<img src="../picture/Februrary%20Fourth/double%20pointer%20process%203.jpg" width = "500" height = "250" alt="process 3" align=center/>

Since the whole process need a loop, therefore we should know when the loop should be ended

<img src="../picture/Februrary%20Fourth/double%20pointer%20loop1.jpg" width = "500" height = "520" alt="loop 1" align=center/>

In this situation, we can see that `curr` is essential in the loop, it always plays `dummy head` role in swap, so when `curr.next = null`,loop stops

But in this linked list, nodes are even, but what about nodes are odd?

<img src="../picture/Februrary%20Fourth/double%20pointer%20loop%202.jpg" width = "400" height = "160" alt="loop 2" align=center/>

Simple, when `curr.next.next = null`, loop stops

```java
ListNode dummy_head = new ListNode(-1);
dummy_head.next = head;
ListNode curr = dummy_head;
ListNode temp;
ListNode temp1;
while(curr.next != null && curr.next.next != null){
    temp = curr.next;
    curr.next = curr.next.next;
    temp1 = curr.next.next;
    curr.next.next = temp;
    temp.next = temp1;
    curr = temp;
}
return dummy_head.next;
```

### Recursion

In this question, recursion is very similar with `divide and conquer`,because it rather focuses on part instead of whole, and using recursion to iterate.

<img src="../picture/Februrary%20Fourth/recursion.jpg" width = "400" height = "160" alt="recursion" align=center/>

This graph largely describes the process of recursion. In every part, there is a `head`and`next`,by using recursion, the `head` of next part is `new`

```java
ListNode new_node = swapPairs(next_node.next);
next_node.next = head;
head.next = new_node;
```

**Plus, do not forget the exit**

```java
if(head == null || head.next == null){
    return head;
}
ListNode next_node = head.next;
ListNode new_node = swapPairs(next_node.next);
next_node.next = head;
head.next = new_node;
return next_node;
```

## Leetcode 19 Nth Node From End of List

[Video Analysis](https://www.bilibili.com/video/BV1vW4y1U7Gf/?spm_id_from=333.788&vd_source=e8f9779956746463d5471d7c18ccae92)

Including:

* Linked List
* Fast-slow pointer
* Dummy head

This question is based on deletion of linked list, so it is very similiar. Actually, it can be solved by using `Doubly linked list`

The reason to select `fast-slow pointer`is because **the speed difference** will `n` distance when `fast` stops, then `slow` is the node we want to delete

**But one thing we should know, to delete one node, the previous node should be noticed first**

<img src="../picture/Februrary%20Fourth/fast-slow%20pointer.jpg" width = "400" height = "280" alt="fast slow pointer" align=center/>

So `fast` will move firstly, moving the N+1 th position, once `fast` reach the position, `slow` begins to move. Two pointers remain the same speed

```java
ListNode dummy_head = new ListNode(-1);
dummy_head.next = head;
ListNode fast = dummy_head;
ListNode slow = dummy_head;
// fast move n + 1 step
for(int i = 0; i <= n; i ++){
    fast = fast.next;
}
while(fast != null){
    fast = fast.next;
    slow = slow.next;
}
slow.next = slow.next.next;
return dummy_head.next;
```

## Leetcode 160 Intersection of Two Linked lists

Including:

* Double Pointers
* Linked List

This question is very similar with **Leetcode 19**, the previous one. Both use double pointers, and meansure the distance

So first, there is no **circle**, which is good.

Here is the method: two pointers first start to move simultaneously, and counting distance, until one pointer reaches to the end. Therefore the gap is `distance1 - distance2`

* if `distance1 - distance2 > 0` means `link 1` is larger than `link 2` , so reset two pointers to initial positions, let `pointer1` to move, to fill the gap. Then moving `pointer1` and `pointer2` together. If `pointer1 == pointer2` means there is an interaction
* if `distance1 - distance2 < 0` means `link 1` is smaller than `link 2` , so reset two pointers to initial positions, let `pointer2` to move, to fill the gap. Then moving `pointer1` and `pointer2` together. If `pointer1 == pointer2` means there is an interaction
* Otherwise, after the reset, moving two pointers together

```java
ListNode currA = headA;
ListNode currB = headB;
int lengthA = 0;
int lengthB = 0;
while(currA != null){
    lengthA ++;
    currA = currA.next;
}

while(currB != null){
    lengthB ++;
    currB = currB.next;
}

int gap = lengthA - lengthB;
currA = headA;
currB = headB;
if(gap > 0){
    for(int i = 0; i < gap; i ++){
        currA = currA.next;
    }
    while(currA != null){
        if(currA == currB){
            return currA;
        }
        currA = currA.next;
        currB = currB.next;
    }
}
else if(gap < 0){
    for(int i = 0; i < -gap; i ++){
        currB = currB.next;
    }
    while(currB != null){
        if(currA == currB){
            return currB;
        }
        currA = currA.next;
        currB = currB.next;
    }
}
else{
    while(currA != null){
        if(currA == currB){
            return currA;
        }
        currA = currA.next;
        currB = currB.next;
    }
}
return null;
```

## Leetcode 141 & 142 Linked List Cycle

Including:

* Fast-slow pointers
* Linked List

To verify whether there is a cycle in linked list, the easiest way is to set `fast-slow pointers`. If there is  a cycle, no matter what the speed difference is, eventually two pointers will meet.

```java
ListNode fast = head;
ListNode slow = head;
while(fast != null && fast.next != null){
    fast = fast.next.next;
    slow = slow.next;
    if(fast == slow){
        return true;
    }
}
return false;
```

Furthermore, to verify the node where the cycle begins if there is no cycle, we can depend on the previous method. After ensuring there is a cycle, then setting a new pointer in the `head`, continuing to move `slow` pointer and `new ` pointer, where two pointers meet, is the node we want to reach

```java
ListNode fast = head;
ListNode slow = head;
while(fast != null && fast.next != null){
    fast = fast.next.next;
    slow = slow.next;
    if(fast == slow){
        ListNode new_pointer = head;
        while(new_pointer != slow){
            new_pointer = new_pointer.next;
            slow = slow.next;
        }
        return new_pointer;
    }
}
return null;
```
