# Day 11 Februrary Eleventh Stack & Queue

## [Leetcode 20 Valid Parentheses](https://leetcode.com/problems/valid-parentheses/)

Including: Stack

*Too easy*

```java
public boolean isValid(String s) {
    if(s.length() % 2 != 0){
        return false;
    }
    ArrayDeque<Character> stack = new ArrayDeque<>();
    for(int i = 0; i < s.length(); i ++){
        char ch = s.charAt(i);
        if(ch == '['){
            stack.push(']');
        }
        else if(ch == '('){
            stack.push(')');
        }
        else if(ch == '{'){
            stack.push('}');
        }
        else if(stack.isEmpty() || stack.peek() != ch){
            return false;
        }
        else{
            stack.pop();
        }
    }
    return stack.isEmpty();
}
```

## [Leetcode 1047 Remove All Adjacent Duplicates In String](https://leetcode.com/problems/remove-all-adjacent-duplicates-in-string/)

Including: Stack

*Too easy*

```java
public static String removeDuplicates(String s) {
    ArrayDeque<Character> stack = new ArrayDeque<>();
    for(int i = 0; i < s.length(); i ++){
        char ch = s.charAt(i);
        if(stack.isEmpty() || stack.peek() != ch){
            stack.push(ch);
        }
        else{
            stack.pop();
        }
    }
    StringBuilder sb = new StringBuilder();
    while(!stack.isEmpty()){
        sb.append(stack.pop());
    }
    sb.reverse();
    return sb.toString();
}
```

## [Leetcode 150 Evaluate Reverse Polish Notation](https://leetcode.com/problems/evaluate-reverse-polish-notation/)

Including：

* Stack
* Medial and suffix expressions

```java
public int evalRPN(String[] tokens) {
    ArrayDeque<Integer> stack = new ArrayDeque<>();
    for(String str : tokens){
        if("+".equals(str)){
            stack.push(stack.pop() + stack.pop());
        }
        else if("-".equals(str)){
            int num1 = stack.pop();
            int num2 = stack.pop();
            stack.push(num2 - num1);
        }
        else if("*".equals(str)){
            stack.push(stack.pop() * stack.pop());
        }
        else if("/".equals(str)){
            int num1 = stack.pop();
            int num2 = stack.pop();
            stack.push(num2 / num1);
        }
        else{
            stack.push(Integer.valueOf(str));
        }
    }
    return stack.pop();
}
```

## Something you should know

[valueOf](https://www.tutorialspoint.com/java/number_valueof.htm)
