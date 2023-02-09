# Day 9 Februrary Nineth String

# [Leetcode 28 Find the Index of the First Occurence in a String](https://leetcode.com/problems/find-the-index-of-the-first-occurrence-in-a-string/description/)

Including:

* Sliding Window
* KMP

## Brute force && Sliding Window

So in the question, we can use sliding window to solve, a little bit brute force

<img src="../picture/Februrary%20Nineth/sliding_window.jpg" width = "400" height = "240" alt="sliding windowm" align=center/>

* Compare `needle` and `haystack` from the beginning
  * if the first letter from both `haystack` and `needle` is identical, then iterate both `needle` and `haystack`
    * if at the end of `needle`, it's all the same, then return true
    * if there is anything different, then return false

```java
public int strStr(String haystack, String needle) {
	int lengthHaystack = haystack.length();
	int lengthNeedle = needle.length();
	// if needle is null, no need to continue
	if(lengthNeedle == 0){
		return 0;
	}
	// if the length of haystack is smaller than needle
	// it is no way that needle could be a substring of haystack
	if(lengthHaystack < lengthNeedle){
		return -1;
	}
	int i = 0;
	int j = 0;
	while(i <= lengthHaystack - lengthNeedle){
		// determine where could we find the initial letter of needle,
		// appears in the haystack
		while(i < lengthHaystack && haystack.charAt(i) != needle.charAt(j)){
			i ++;
		}
		// if in the end, we could not find the identical initial letter from the haystack
		if(i == lengthHaystack){
			return -1;
		}
		i ++;
		j ++;
		while(i < lengthHaystack && j < lengthNeedle && haystack.charAt(i) == needle.charAt(j)){
			i ++;
			j ++;
		}
		if(j == lengthNeedle){
			return i - j;
		}
		else{
			i = i - j + 1;
			j = 0;
		}
	}
	return -1;
}
```

## KMP

# [Leetcode 459 Repeated Substring Pattern](https://leetcode.com/problems/repeated-substring-pattern/)

Including:

* Sliding Windows
* Built-in

1. If the number of letter is less or equal to 1, then return false;
2. Set one `ArrayList`
3. Get every possible interval of the string, and store in `ArrayList`
   1. Because there are at least two substring, so one's length must smaller than `s.length / 2`
4. Slice the string, intervals are chosen from `ArrayList`, and compare each substring

```java
public boolean repeatedSubstringPattern(String s) {
    if(s.length() <= 1){
        return false;
    }
    ArrayList<Integer> list = new ArrayList<>();
    for(int i = 1; i <= s.length() / 2; i ++){
        if(s.length() % i == 0){
            list.add(i);
        }
    }
    for(int integer : list){
        String temp = s.substring(0, integer);
        for(int j = integer; j < s.length(); j += integer){
            if(!s.substring(j, j + integer).equals(temp)){
                break;
            }
            if(j + integer == s.length()){
                return true;
            }
        }
    }
    return false;
}
```
