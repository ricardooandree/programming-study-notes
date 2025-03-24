<font color="#7f7f7f"><em>Friday, 01-11-2024 - 15:28</em></font>
#programming #dsa 

## Easy Difficulty
### Remove Outermost Parentheses
String input/output examples:
- `(()())(())` -> `()()()`
- `()()` -> ` `
- `((()))` -> `(())`

Intuition: Use a counter 'level' as you advance to inner and outer parentheses. If you're in a level that's not the outermost one, you can copy the string.
```java
public static String removeOuterParentheses(String str){
	StringBuilder result = new StringBuilder();
	int level = 0;

	for (char c : str.toCharArray()){
		if (c == '(')){
			if (level > 0){ result.append(c); }  // ignore the first outermost (
			level++;
			
		}
		else if (c == ')'){
			level--;
			if (level > 0){ result.append(c); }  // ignore the last outermost )
			
		}
	}
	return result.toString();
}
```
**TIME  COMPLEXITY:** O(N)
**SPACE COMPLEXITY:** O(N)

---
### Largest Odd Number in String
Given a string number, representing a large integer. Return the largest-valued off integer (as a string) that is non-empty substring of number, or an empty string if no odd integer exists.

A substring is a contiguous sequence of characters within a string.

Example 1
- `num = 52` -> `5`, the only non-empty substrings are: 5, 2, 52.
- `num = 4206` -> ` `
- `num = 35427` -> `35427`

Intuition: The number is odd if the last number of it is odd. For it to be the largest we need the maximum amount of characters in it so we start iterating from right to left.

NOTE: Using division by 10 isn't the best solution due to overflowing.
NOTE: Remember there's methods such as string.length() and string.charAt(index) at our disposal.
```java
public static String largestOddNumber(string num){
	for (int i = num.length() - 1; i >= 0; i--) {
		if ((num.charAt(i) - '0') % 2 != 0) {
			return num.substring(0, i + 1);
		}
	}
	return "";
}
```
**TIME  COMPLEXITY:** O(N)
**SPACE COMPLEXITY:** O(1)

---
### Longest Common Prefix
Write a function to find the longest common prefix string amongst an array of strings. If there is no common prefix, return an empty string "".

Examples
	Input: `strings = ["flower", "flow", "flight"]`
	Output: "fl"

```java
public String longestCommonPrefix(String[] strs) {
	StringBuilder result = new StringBuilder();

	int minSize = strs[0].length();
	for (int i = 1; i < strs.length; i++){
		if (strs[i].length() < minSize){
			minSize = strs[i].length();
		}
	}

	for (int i = 0; i < minSize; i++){
		char c = strs[0].charAt(i);
		
		for (int j = 1; j < strs.length; j++){
			if (strs[j].charAt(i) != c){
				return result.toString();
			}
		}

		result.append(c);
	}
	return result.toString();
}
```
**TIME  COMPLEXITY:** O(NM), N-Number of strings, M-Prefix size
**SPACE COMPLEXITY:** O(M)

---
### Isomorphic Strings
Two strings are isomorphic if the characters can be replaced to form the other strings -> hash code premise.

The same character can't map two different characters.

NOTE: It's important to have two hash maps since one can be correct and the other not originating a false positive.
```java
public boolean isIsomorphic(String s, String t) {
	if (s.length() != t.length()){ return false; }

	HashMap<Character,Character> mapS = new HashMap<>();
	HashMap<Character,Character> mapT = new HashMap<>();

	for (int i = 0; i < s.length(); i++) {
		char charS = s.charAt(i);
		char charT = t.charAt(i);

		if (mapS.containsKey(charS) && mapS.get(charS) != charT) {
			return false;
		}
		if (mapT.containsKey(charT) && mapT.get(charT) != charS) {
			return false;
		}

		mapS.put(charS, charT);
		mapT.put(charT, charS);
	}
	return true;
}
```
**TIME  COMPLEXITY:** O(N) or O(N^2) in case of hash map collisions
**SPACE COMPLEXITY:** O(2N)

---
### Maximum Nesting Depth of the Parentheses
**Input:** s = "(1)+((2))+(((3)))"
**Output:** 3

**Explanation:** Digit 3 is inside of 3 nested parentheses in the string.
```java
public int maxDepth(String s){
	int depth = 0, max = 0;

	for (char c : s.toCharArray()){
		if (c == '('){
			depth++;
		}
		else if (c == ')'){
			depth--;
		}

		if (depth > max){ max = depth; }
	}
	return max;
}
```
**TIME  COMPLEXITY:** O(N)
**SPACE COMPLEXITY:** O(1)

---
### Roman to Integer

| Symbol | Value |
| ------ | ----- |
| I      | 1     |
| V      | 5     |
| X      | 10    |
| L      | 50    |
| C      | 100   |
| D      | 500   |
| M      | 1000  |
Note that IV = 4, IX = 9, XL = 40, XC = 90, CD = 400, CM = 900.

```java
public int static romanToInteger(String s){
	HashMap<Character, Integer> romanMap = new HashMap<>();
	romanMap.put('I', 1);
	romanMap.put('V', 5);
	romanMap.put('X', 10);
	romanMap.put('L', 50);
	romanMap.put('C', 100);
	romanMap.put('D', 500);
	romanMap.put('M', 1000);

	int sum = 0, int previousValue = 0;
	for (int i = s.length() - 1; i >= 0; i--){
		int currentValue = romanMap.get(s.charAt(i));

		if (currentValue < previousValue){
			sum -= currentValue;
		}
		else {
			sum += currentValue;
		}

		previousValue = currentValue;
	}
	
	return sum;
}
```
**TIME  COMPLEXITY:** O(N)
**SPACE COMPLEXITY:** O(1) - HashMap of fixed size

---
---

## Medium Difficulty
### Reverse Words in a String

---
### Valid Anagram
A two Hash Maps approach is unnecessary and will fail some use cases where they strings are too large.

##### 1. Frequency Hash Map or Array
Use a Hash Map to count the frequency of the characters and subtract when iterating the second string, at the end the size of the map should be empty.

NOTE: You can use an array hash to add/subtract if its only lower-case letters.

```java
public boolean isAnagram(String s, String t) {
	if (s.length() != t.length()) { return false; }

	HashMap<Character, Integer> freq = new HashMap<>();
	for (char c : s.toCharArray()) {
		freq.put(c, freq.getOrDefault(c, 0) + 1);
	}

	for (char c : t.toCharArray()) {
		if (!freq.containsKey(c)){
			return false;
		}

		freq.put(c, freq.get(c) - 1);

		if (freq.get(c) == 0){
			freq.remove(c);
		}
	}
	return freq.isEmpty();
}
```
**TIME  COMPLEXITY:** O(2N)
**SPACE COMPLEXITY:** O(1) 

##### 2. Sorting and Comparing
Consists of transforming the strings into character arrays and then sorting them and check if they're equal.
**TIME  COMPLEXITY:** O(NLOG N)
**SPACE COMPLEXITY:** O(N) - toCharArray() creates an extra array

---
### Rotate String
Summing up the initial string twice generates a string with all possible rotations.

```java
public boolean rotateString(String s, String goal) {
	if (s.length() != goal.length()){ return false; }

	String concatString = s + s;
	for (int i = 0; i < concatString.length() - s.length(); i++){
		String substring = concatString.substring(i, i + s.length());

		if (substring.equals(goal)){
			return true;
		}
	}
	return false;
}
```
**TIME  COMPLEXITY:** O(N^2) - For loop + equals method
**SPACE COMPLEXITY:** O(2N) - Concatenation of two strings

##### 2. Simplified Approach
```java
public boolean rotateString(String s, String goal) {
	return s.length() == goal.length() && (s + s).contains(goal);
}
```
**TIME  COMPLEXITY:** O(3N) - Concatenation O(N) + equals O(2N)
**SPACE COMPLEXITY:** O(2N) - Concatenation of two strings

---
