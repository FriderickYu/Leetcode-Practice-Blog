# Day 10 Februray Tenth Stack & Queue

## [Leetcode 232 Implement Queue using Stacks](https://leetcode.com/problems/implement-queue-using-stacks/description/)

Including:

* Built-in
* Stack

There are several functions we should implement

* `MyQueue()`
* `push(x)`
* `pop()`
* `peek()`
* `empty()`

We will use `ArrayDeque` to construct this `Queue`, here are some usual built-in structure in `Java`

* `ArrayDeque`
* `LinkedList`
* `Stack`
* `Deque`

`push` and `empty` we could use basic built-in function, like `isEmpty(), push()`

But `peek(), pop()` are more different

<img src="../picture/Februrary%20Tenth/stack_to_queue.jpg" width = "300" height = "220" alt="construct queue by stack" align=center/>

`pop()`:

* whether `stackOut` is empty or not
  * if yes, pop all elements from `stackIn`, and push these into `stackOut`
  * if no, simply pop element from `stackOut`

`peek()`:

* whether `stackOut` is empty or not
  * if yes, pop all elements from `stackIn`, and push these into `stackOut`
  * if no, simply use `peek()` from `stackOut`

```java
public int pop() {
    if(!stackOut.isEmpty()){
        return stackOut.pop();
    }
    while(!stackInt.isEmpty()){
        stackOut.push(stackInt.pop());
    }
    return stackOut.pop();
}

public int peek() {
    if(!stackOut.isEmpty()){
        return stackOut.peek();
    }
    while(!stackInt.isEmpty()){
        stackOut.push(stackInt.pop());
    }
    return stackOut.peek();
}
```

## [Leetcode 225 Implement Stack using Queues](https://leetcode.com/problems/implement-stack-using-queues/description/)

Including:

* Built-in
* Queue

### Double Queues

This is quite different with the previous question. In this part, using double queues is to use another queue as a backup, not in and out.

```java
class MyStack {
    ArrayDeque<Integer> queue1;
    ArrayDeque<Integer> queue2;
    public MyStack() {
        queue1 = new ArrayDeque<>();
        queue2 = new ArrayDeque<>();
    }

    public void push(int x) {
        queue2.offer(x);
        while(!queue1.isEmpty()){
            queue2.offer(queue1.poll());
        }
        ArrayDeque<Integer> queueTemp;
        queueTemp = queue1;
        queue1 = queue2;
        queue2 = queueTemp;
    }

    public int pop() {
        return queue1.poll();
    }

    public int top() {
        return queue1.peek();
    }

    public boolean empty() {
        return queue1.isEmpty();
    }
}
```

### Just One Queue

This method is very simple, just put these elements which are before the last element

<img src="../picture/Februrary%20Tenth/queue_to_stack.jpg" width = "300" height = "180" alt="construct stack by queue" align=center/>

* For instance, 4 is the element we want to pop, which is the last one
* There are three elements before 4, so just copying them and insert into `queue`, then `pop` these elements
* we can set a `count` set determine whether this is the element we want to `pop`

```java
class MyStack {
    Deque<Integer> deque;
    public MyStack() {
        deque = new ArrayDeque<>();
    }

    public void push(int x) {
        deque.addLast(x);
    }

    public int pop() {
        int size = deque.size();
        while(size > 1){
            deque.addLast(deque.peekFirst());
            deque.pollFirst();
            size --;
        }
        return deque.pollFirst();
    }

    public int top() {
        return deque.peekLast();
    }

    public boolean empty() {
        return deque.isEmpty();
    }
}
```
