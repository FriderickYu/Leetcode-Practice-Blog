# Day 6 February Sixth

## Hash Table

**When we have to quickly determine whether an element appear in a set, then consider Hash** -> Time Complexity Optimal

Actually, the easiest hash table is array, imaging index of array is `key`,

and element is `value`, normally we can use index to get the element directly

In `array` search is quick, and insertion and deletion are slow; in `linked list` search is slow, but insertion and deletion are quick. `Hash table` are compatible with the advantages of `array` and `linked list`, faster in both search and insertion/deletion. `Hash table` is made up of the combination of `array` and `linked list`

`Hash table` uses `hash function` to map `key` into `index` of `hash table`, and then the corresponding `value` is founded by querying the `index`

<img src="../picture/February%20Sixth/hash%20table.png" width = "500" height = "270" alt="hash table" align=center/>

For instance, three `entry set` `("ytq", 98), ("qty", 89), ("ddk", 80)`

`Hash table` will use `hash function` to calculate `key`, to produce its `index`, like $index = hashFunction(ytq)$, $hashFunction = hashCode(key)\%tableSize$

The reason why uses `mod` is allowing all `key` map into `tableSize` area

However, if `entry set` is too many, exceeds `tableSize`, then it is possible that `ddk` and `ytq` will be mapped into same index, this is called `hash collision`

### Zip

The perfect example is the above image, when `ddk` and `ytq` are mapped into the same index. Remember, `hash table` combines `array` and `linked list`, therefore, we can link `ddk` behind `ytq`. Notice, this method may waste memory, therefore `tableSize` need to be appropriate

### Quadratic Probing

This one is more easier than the previous one. If you find hash collision, just move another `entry set` to the next index. But this requires `hash table` size larger than `array`

**Warning**

The insertion of `hash table` is disordered. It means when you input something in `hash table`, like `("ytq", 98), ("qty", 89), ("ddk", 80)`, when you want to display, the output order may be `("ddk", 80), ("ytq", 98), ("qty", 89)`. The flaw is, to make it fast to search, it sacrifices `memory`

In practice, three options are considerde when we want to use `hash table` to solve the problem, `array`, `set`, `map`; In Java, it's `HashSet`, `TreeMap`, `HashMap`

## [Leetcode 242 Valid Anagram](https://leetcode.com/problems/valid-anagram/description/)

Including:

* Array
* Hash Table

### Hash Table

Basically, this question is to determine whether an element in a set. So `hash table`

Create a `hash table` and put every element in the first string. `key` is the `char` and `value` is the frequency

`map.getOrDefault(c1, 0)` means, if `c1` exists, then return to `value`, however, if it does not, return to the default value, `0`

Iterating another string, if the `char` exists in `hash table`, then `-1`. In the end, if the frequency reduces to 1, then remove this `entry set`. Howevr, if the `char` does not exist in `hash table`, then means this element does not exist in the anagram, return to `false` directly

```java
public boolean isAnagram(String s, String t) {
HashMap<Character, Integer> map = new HashMap<>();
for(int i = 0; i < s.length(); i ++){
    char c1 = s.charAt(i);
    map.put(c1, map.getOrDefault(c1, 0) + 1);
}
for(int j = 0; j < t.length(); j ++){
    char c2 = t.charAt(j);
    if(map.get(c2) != null){
        if(map.get(c2) == 1){
            map.remove(c2);
        }
        else{
            map.put(c2, map.get(c2) - 1);
        }
    }
    else{
        return false;
    }
}
return map.isEmpty();
}
```

### Array

As we said, `array` is the easiest form of `hash table`. Therefore, we can also solve this problem by `array`

Here is one thing, there are only lowercase letters in `anagram`. And letters are consistent, presetned in ASCII code (97~122). And numbers are sured, 26 lowercase letters. Therefore, index is the letter, and what array stores is the frequency, if one letter appears once, then `+1` in the corresponding index

```java
public boolean isAnagram(String s, String t) {
int[] record = new int[26];
for(int i = 0; i < s.length(); i ++){
    record[s.charAt(i) - 'a'] ++;
}
for(int j = 0; j < t.length(); j ++){
    record[t.charAt(j) - 'a'] --;
}
for(Integer integer : record){
    if(integer != 0){
        return false;
    }
}
return true;
}
```

## [Leetcode 349 Intersection of Two Arrays](https://leetcode.com/problems/intersection-of-two-arrays/)

Including:

* Hash table
* Set

*Too basic*

```java
HashSet<Integer> set = new HashSet<>();
HashSet<Integer> result = new HashSet<>();
for(Integer integer : nums1){
    set.add(integer);
}
for(Integer integer2 : nums2){
    if(set.contains(integer2)){
        result.add(integer2);
    }
}
return result.stream().mapToInt(x -> x).toArray();
```

## [Leetcode 1 Two Sum](https://leetcode.com/problems/two-sum/)

*Too basic*

```java
HashMap<Integer, Integer> map = new HashMap<>();
int[] result = new int[2];
for(int i = 0; i < nums.length; i ++){
    if(map.containsKey(target - nums[i])){
        result[0] = i;
        result[1] = map.get(target - nums[i]);
    }
    map.put(nums[i], i);
}
return result;
```

## [Leetcode 202 Happy Number](https://leetcode.com/problems/happy-number/)

Including:

* Set
* Mod

This question is very tricky, need to think thoroughly

First, how to generate the process

$19 = 1^2 + 9^2, 82 = 8^2 + 2^2$

This can be solved by using `mod`

$19 \div 10 =1...9, 1 / 10 = 1$

```java
public int generateNextNumber(int n){
int result = 0;
while(n > 0){
    int temp = n % 10;
    result += temp * temp;
    n = n / 10;
}
return result;
}
```

To determine whether it is the `happy number`, first in the end `n` should be 1. Second, if `n` is not the `happy number`, **it will
iterate endlessly**. But how to determine this will iterate forever?

So, if `n` appears for more than one time, then it will iterate forever. We can use `set` to store all results

```java
public boolean isHappy(int n) {
HashSet<Integer> set = new HashSet<>();
while(n != 1 && set.contains(n) == false){
    set.add(n);
    n = generateNextNumber(n);
}
return n == 1; 
}
```
