# Day 8 Februrary Eighth

## [Leetcode 344 Reverse String](https://leetcode.com/problems/reverse-string/)

Including:

* String and char array
* Double Pointers

*Too easy*

```java
public void reverseString(char[] s) {
    int left = 0;
    int right = s.length - 1;
    while(left < right){
        Solution.swap(s, left, right);
        right --;
        left ++;
    }
}
public static void swap(char[] s, int i, int j){
    char c = s[i];
    s[i] = s[j];
    s[j] = c;
}
```

## [Leetcode 541 Reverse String II](https://leetcode.com/problems/reverse-string-ii/)

Including:

* Simulation
* Double Pointers
* String and char array

There are several issues should be noticed:

1. reverse the first `k` characters for every `2k` characters
2. if fewer than `k` character left, reverse all rest of them
3. if less than `2k` but greater than or equal to `k`, then reverse the first `k` and leave the rest be

* Do not use `i ++` as the usual, use ` i += 2 * k` instead
* To determine whether the number of rest elements are equal to k or not, compare `length` and `left + k - 1`

```java
char[] c = s.toCharArray();
    for(int i = 0; i < c.length; i += 2 * k){
        int left = i;
        int right = Math.min(c.length - 1, left + k - 1);
        Solution.reverse(c, left, right);
    }
    return new String(c);
}
public static char[] reverse(char[] s, int left, int right){
    while(left < right){
        Solution.swap(s, left, right);
        left ++;
        right --;
    }
    return s;
}
public static void swap(char[] s, int i, int j){
    char c = s[i];
    s[i] = s[j];
    s[j] = c;
}
```

## Replace the space with %20

Including:

* Iteration
* Double Pointers
* Built-in
* StringBuilder and StringBuffer

### Built-in

```java
public static String method1(String s){
    return s.replaceAll(" ", "%20");
}
```

### Traditional iteration

```java
public static String method2(String s){
    StringBuilder result = new StringBuilder();
    for(int i = 0; i < s.length(); i ++){
        char c = s.charAt(i);
        if(c == ' '){
            result.append("%20");
        }
        else{
            result.append(c);
        }
    }
    return new String(result);
}
```

### Double Pointers

* Expanding the original array, when meet one space, then creating two extra spaces
* Appending these extra spaces to the end of array
* From end to start, `left` points the end of original array, `right` points the end of expanded array
* When `left` meets a space, create `'0', '2', '%'` by right
* Otherwise, `arr[right] = arr[left]`, to fulfill the array

```java
public static String method3(String s){
    if(s == null || s.length() == 0){
        return s;
    }
    StringBuilder string = new StringBuilder();
    for(int i = 0; i < s.length(); i ++) {
        if (s.charAt(i) == ' ') {
            string.append("  ");
        }
    }
    if(string.length() == 0){
        return s;
    }
    int left = s.length() - 1;
    s += string.toString();
    int right = s.length() - 1;
    char[] chars = s.toCharArray();
    while(left >= 0){
        if(chars[left] == ' '){
            chars[right --] = '0';
            chars[right --] = '2';
            chars[right] = '%';
        }
        else{
            chars[right] = chars[left];
        }
        left --;
        right --;
    }
    return new String(chars);
}
```

## [Leetcode 151 Reverse Words in a String](https://leetcode.com/problems/reverse-words-in-a-string/submissions/893952401/)

Including:

* Built-in
* Double Pointers

### Built-in

If we do not care about algorithm practice and extra data structure, only use built-in function will be a rational choice

1. filter all spaces at the start and end
2. Use `regex` to filter all continuous spaces, and convert `String` to `List`, `\\s+` means match any blank characters
3. reverse `list`
4. use `join` to append all elements in the list with a space

```java
s = s.trim();
List<String> list = Arrays.asList(s.split("\\s+"));
Collections.reverse(list);
return s.join(" ", list);
```

### Double Pointers

If we reverse all letters from the string, and re-organize each word, then it will achieve the purpose

1. Delete abundent spaces
2. reverse the entire string
3. reverse each word

For instance, `"the sky  is blue "`

* Delete abundent spaces, `"the sky is blue"`
* reverse the entire string, `"eulb si yks eht"`
* reverse each word, `"blue is sky the"`

#### Delete abundent spaces

1. Deleting all the start and end spaces
2. Determine whether a space should append in the `StringBuilder`
   1. There is a space **OR**
   2. In the end of `StringBuilder`, it is not a space

```java
public StringBuilder removeSpace(String s){
  int start = 0;
  int end = s.length() - 1;
  // filter the start space and end space
  while(s.charAt(start) == ' '){
      start ++;
  }
  while(s.charAt(end) == ' '){
      end --;
  }
  StringBuilder sb = new StringBuilder();
  //  To add a space into StringBuilder, needs to fit two requirements
  //  1. There is a space
  //  2. In the end of StringBuilder, it is not a space
  while(start <= end){
      char c = s.charAt(start);
      if(c != ' ' || sb.charAt(sb.length() - 1) != ' '){
      sb.append(c);
  }
  start ++;
  }
  return sb;
}
```

#### Reverse the entire string

Very similar with [reverse string](https://leetcode.com/problems/reverse-string/)

```java
// reverse string from start to end
public void reverseString(StringBuilder sb, int start, int end){
  while(start < end){
      char temp = sb.charAt(start);
      sb.setCharAt(start, sb.charAt(end));
      sb.setCharAt(end, temp);
      start ++;
      end --;
  }
}
```

#### Reverse each word

1. Needs to ensure where is the range, start and end
2. `end ++` until there is a space

```java
public void reverseWord(StringBuilder sb){
  int start = 0;
  int end = 1;
  int n = sb.length();
  while(start < n){
      while(end < n && sb.charAt(end) != ' '){
          end ++;
      }
      reverseString(sb, start, end - 1);
      start = end + 1;
      end = start + 1;
  }
}
```

## Rotate String

Including:

* Built-in function
* Double Pointers -> Reverse

### Built-in

```java
public String reverseLeftWords(String s, int n) {
  StringBuilder sb = new StringBuilder();
  for(int i = n; i < s.length(); i ++){
      sb.append(s.charAt(i));
      if(n <= 0){
          return sb.toString();
      }
  }
  for(int j = 0; j < n; j ++){
      sb.append(s.charAt(j));
  }
  return sb.toString();
}
```

### Double Pointes && Reverse

This question is very similar with reverse string, watch carefully, `"abcdefg", k=2"`

1. Reverse `0~n-1` `bacdefg`
2. Reverse `n~s.length()-1` `bagfedc`
3. Revese the entire string `cdefgab`

```java
public String reverseLeftWords(String s, int n) {
  StringBuilder sb = new StringBuilder(s);
  reverse(sb, 0, n - 1);
  reverse(sb, n, s.length() - 1);
  return sb.reverse().toString();
}

public void reverse(StringBuilder sb, int start, int end){
  while(start < end){
      char temp = sb.charAt(start);
      sb.setCharAt(start, sb.charAt(end));
      sb.setCharAt(end, temp);
      start ++;
      end --;
  }
}
```
