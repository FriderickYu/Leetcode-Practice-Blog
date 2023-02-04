# Day 3 February Third

## Linked List

There are four kinds of Linked List we always need in algorithms:

* Single Linked List
* Doubly Linked List
* Circular Linked List

Different from `array`, `linked list` is not sequential in the memory,
this is good for insertion and deletion, because `array` is a sequential
data structure, therefore when you want to insert/delete one element in
`array`, then you need to move all elements behind this one. However, `linked list`
uses pointer to link all nodes, only thing we need to do is point to element we want to

This shows how to delete a node in `linked list`

<img src="../picture/Februrary%20Third/linked%20list%20deletion.jpg" width = "300" height = "200" alt="linked list deletion" align=center/>

But what if the node we want to delete, is head? There is no previous node of head, therefore we cannot use old method

It is rather simple, we just move `head` to the next node, then the next node will become new `head`

```java
head = head.next;
return head;
```

This shows how to add a node in `linked list`

<img src="../picture/Februrary%20Third/linked%20list%20insertion.jpg" width = "300" height = "200" alt="linked list deletion" align=center/>

## Leetcode 203 Remove Linked List Elements

[Video Analysis](https://www.bilibili.com/video/BV18B4y1s7R9/?spm_id_from=333.788&vd_source=e8f9779956746463d5471d7c18ccae92)

Including:

* Linked List
* Dummy head
* Operations on Linked List

### Dummy head

Deleting nodes is not a hard thing, but one thing should be noticed, `head` may be the node we wan to delete. **When we use `dummy head`? If we want to operate nodes, but we do not want to verify whether this node is `head` or not, then `dummy head` is the most suitable choice**

so first we define `dummy_head` point to `head`. After that, define two pointers, `previous`and`current`, that is because

the precondition is to remember <font color="red">the previous node</font>, so `previous` points to the previous node, and `current`records
the current node

```java
ListNode dummy_head = new ListNode(-1, head);
ListNode previous_pointer = dummy_head;
ListNode current_pointer = head;
if(head == null){
    return head;
}
while(current_pointer != null){
    if(current_pointer.val == val){
        previous_pointer.next = current_pointer.next;
    }
    else{
        previous_pointer = current_pointer;
    }
    current_pointer = current_pointer.next;
}
return dummy_head.next;
```

Another issue need to attention, since `head` may be deleted, so we should return `dummy_head.next`

### Traditional method

Same issue as the previous method, `head` should be considered well

First, we should consider that head is the element we want to delete, if it is, then move the `head`

Second, we should consider two situations, one is there is no element, `return null`, or like `[1, 1, 1, 1]` `head` just move to the end.

```java
// considering that head is deleted
while(head != null && head.val == val){
    head = head.next;
}
if(head == null){
    return head;
}
ListNode pre = head;
ListNode cur = head.next;
while(cur != null){
    if(cur.val == val){
        pre.next = cur.next;
    }
    else{
        pre = cur;
    }
    cur = cur.next;
}
return head;
```

## Leetcode 707 Design Linked List

[Video Analysis](https://www.bilibili.com/video/BV1FU4y1X7WD/?spm_id_from=333.788&vd_source=e8f9779956746463d5471d7c18ccae92)

Including:

* Linked List
* Doubly Linked List

### Construction of Nodes

```java
class ListNode{
    int value;
    ListNode next;
    ListNode(){}
    ListNode(int value){
        this.value = value;
    }
    ListNode(int value, ListNode next){
        this.value = value;
        this.next = next;
    }
}
```

### Initialization

Virtual head is needed, and size(to help count how many nodes)

```java
int size;
    ListNode dummy_head;
    public MyLinkedList() {
        size = 0;
        dummy_head = new ListNode(0);
    }
```

### Search by Index

First verify index position is legal or not

Second using pointer to traverse the `LinkedList`

```java
public int get(int index) {
// verify index is legal or not
if(index < 0 || index > size - 1){
    return -1;
}
ListNode current_node = dummy_head;
// because it begins with dummy_head, so it is index+1
for(int i = 0; i <= index; i ++){
    current_node = current_node.next;
}
return current_node.value;
}
```

### Insertion

```java
if(index > size){
    return;
}
if(index < 0){
    index = 0;
}
size ++;
// first, find the previous node of index node
ListNode previous_node = dummy_head;
for(int i = 0; i < index; i ++){
    previous_node = previous_node.next;
}
ListNode new_node = new ListNode(val);
new_node.next = previous_node.next;
previous_node.next = new_node;
```

### Deletion

```java
if(index < 0 || index > size - 1){
    return;
}
size --;
// delete first node, moving dummy_head
if(index == 0){
    dummy_head = dummy_head.next;
    return;
}
ListNode previous_node = dummy_head;
for(int i = 0; i < index; i ++){
    previous_node = previous_node.next;
}
previous_node.next = previous_node.next.next;
```

### Supplement

Considering the requirement of add elements at the end, therefore using `Doubly linked list`will be more efficient

```java
//双链表
class ListNode{
    int val;
    ListNode next,prev;
    ListNode() {};
    ListNode(int val){
        this.val = val;
    }
}


class MyLinkedList {  

    //记录链表中元素的数量
    int size;
    //记录链表的虚拟头结点和尾结点
    ListNode head,tail;
  
    public MyLinkedList() {
        //初始化操作
        this.size = 0;
        this.head = new ListNode(0);
        this.tail = new ListNode(0);
        //这一步非常关键，否则在加入头结点的操作中会出现null.next的错误！！！
        head.next=tail;
        tail.prev=head;
    }
  
    public int get(int index) {
        //判断index是否有效
        if(index<0 || index>=size){
            return -1;
        }
        ListNode cur = this.head;
        //判断是哪一边遍历时间更短
        if(index >= size / 2){
            //tail开始
            cur = tail;
            for(int i=0; i< size-index; i++){
                cur = cur.prev;
            }
        }else{
            for(int i=0; i<= index; i++){
                cur = cur.next; 
            }
        }
        return cur.val;
    }
  
    public void addAtHead(int val) {
        //等价于在第0个元素前添加
        addAtIndex(0,val);
    }
  
    public void addAtTail(int val) {
        //等价于在最后一个元素(null)前添加
        addAtIndex(size,val);
    }
  
    public void addAtIndex(int index, int val) {
        //index大于链表长度
        if(index>size){
            return;
        }
        //index小于0
        if(index<0){
            index = 0;
        }
        size++;
        //找到前驱
        ListNode pre = this.head;
        for(int i=0; i<index; i++){
            pre = pre.next;
        }
        //新建结点
        ListNode newNode = new ListNode(val);
        newNode.next = pre.next;
        pre.next.prev = newNode;
        newNode.prev = pre;
        pre.next = newNode;
  
    }
  
    public void deleteAtIndex(int index) {
        //判断索引是否有效
        if(index<0 || index>=size){
            return;
        }
        //删除操作
        size--;
        ListNode pre = this.head;
        for(int i=0; i<index; i++){
            pre = pre.next;
        }
        pre.next.next.prev = pre;
        pre.next = pre.next.next;
    }
}

```

## Leetcode 206 Reverse Linked List

[Video Analysis](https://www.bilibili.com/video/BV1nB4y1i7eL/?spm_id_from=333.788&vd_source=e8f9779956746463d5471d7c18ccae92)

Including:

* Linked List
* Double pointers
* Recursion

### Double Pointers

This method is the most easy way to simulate whole process

`pre`records the previous node, in the beginning, it presents`null`

`curr`records the current node

Actually, this method is more like triple pointers, because we need one extra pointer to record

`curr.next`. Once `curr.next=pre`then we will lost next node

<img src="../picture/Februrary%20Third/reverse%20linked%20list(double%20pointers).jpg" width = "500" height = "200" alt="reverse" align=center/>

```java
ListNode pre = null;
ListNode curr = head;
ListNode then = null;
while(curr != null){
    then = curr.next;
    curr.next = pre;
    pre = curr;
    curr = then;
}
return pre;
```

### Recursion

This method is very similar with `Double Pointers`,except that this uses recursion to replace iteration

In **recursion**, the most important thing is to set exit, when the recursion stops

We already knew when`curr=null`,so this is the exit

```java
public ListNode reverseList(ListNode head) {
    return reverse(null, head);
}
private ListNode reverse(ListNode pre, ListNode curr){
    if(curr == null){
        return pre;
    }
    ListNode then = null;
    then = curr.next;
    curr.next = pre;
    return reverse(curr, then);
}
```
