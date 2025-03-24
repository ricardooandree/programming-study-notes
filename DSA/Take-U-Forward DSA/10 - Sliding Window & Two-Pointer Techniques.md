<font color="#7f7f7f"><em>Friday, 01-11-2024 - 15:29</em></font>

## Index
- [[#Theoretical Fundaments]]
- [[#Exercises]]

## Theoretical Fundaments
[[03 - Array Problems#Find the maximum sum of a subarray of size K |Fixed Sliding Window Example]]
[[03 - Array Problems#Smallest subarray with sum >= K|Dynamic Sliding Window Example]]

---
---
## Exercises
### Longest substring without repeating characters
Given a string `s`, find the length of the **longest substring** without duplicate characters.

**Example 1:**
**Input:** s = "abcabcbb"
**Output:** 3
**Explanation:** The answer is "abc", with the length of 3.

##### 1. Solid Optimal Algorithm
```java
public int lengthOfLongestSubstring(String s) {
	HashMap<Character, Integer> charMap = new HashMap<>();
	int maxLen = 0;
	int left = 0, right = 0;

	while (right < s.length()){
		char c = s.charAt(right);

		if (charMap.containsKey(c)){     // end of valid substring
			if (right - left > maxLen){  // update max substring length
				maxLen = right - left;
			}

			charMap.remove(s.charAt(left));
			left++;
			
		} else {
			charMap.put(c, right);
			right++;
		}  
	}
	
	if (right - left > maxLen){  // in case the max substring is the at the last
		return right - left;
	}
	
	return maxLen;
}
```
<u>TIME COMPLEXITY</u>:  O(N)
<u>SPACE COMPLEXITY</u>: O(M) - M is the number of possible characters

##### 2. Slightly Optimised Algorithm
Instead of moving the left pointer one by one, jump it to the last index of the current character. But needs careful to not move the pointer before what it currently is already.
```java
public int lengthOfLongestSubstring(String s) {
	HashMap<Character, Integer> charIndexMap = new HashMap<>();
	int maxLen = 0;
	int left = 0, right = 0;

	while (right < s.length()){
		char c = s.charAt(right);

		if (charIndexMap.containsKey(c)){  // end of valid substring
			int charIndex = charIndexMap.get(c);
			
			if (charIndex + 1 > left){
				left = charIndex + 1;
			}
		}

		charIndexMap.put(c, right);
		right++;

		if (right - left > maxLen){  // update max substring length
			maxLen = right - left;
		}
	}

	return maxLen;
}
```
<u>TIME COMPLEXITY</u>:  O(N)
<u>SPACE COMPLEXITY</u>: O(M) - M is the number of possible characters

---
### Max Consecutive Ones III
Given a binary array `nums` and an integer `k`, return _the maximum number of consecutive_ `1`_'s in the array if you can flip at most_ `k` `0`'s.

**Input:** nums = [0,0,1,1,0,0,1,1,1,0,1,1,0,0,0,1,1,1,1], k = 3
**Output:** 10
**Explanation:** [0,0,1,1,**1**,**1**,1,1,1,**1**,1,1,0,0,0,1,1,1,1]
Bolded numbers were flipped from 0 to 1. The longest subarray is underlined.

**-> Find the longest subarray with at most `k` zeros.**

INTUITION: Increase the window until you have more zeros than allowed, after that move the left pointer until you hit a zero and move past it, this way you don't need to "break the window".

> [!Danger] NOTE
> In this implementation of a dynamic sliding window we might need to move the left pointer lots of times at once hence why we need the internal while loop. 
> On the previous exercise we did not need it because we used a map to jump the left pointer.

##### 1. Approach with double while - O(2N)
```java
public int longestOnes(int[] nums, int k){
	int left = 0, right = 0;
	int maxWindow = 0, zeroCounter = 0;

	while (right < nums.length){
		if (nums[right] == 0){
			zeroCounter++;
		}

		while (zeroCounter > k){
			if (nums[left] == 0){
				zeroCounter--;
			}

			left++;
		}

		int len = right - left + 1;
		if (len > maxWindow){
			maxWindow = len;
		}

		right++;
	}

	return maxWindow;
}
```
<u>TIME COMPLEXITY</u>:  O(2N)
<u>SPACE COMPLEXITY</u>: O(1)

##### 2. Approach without double loop - not O(N) but not O(2N)
Another approach focuses on increasing the window until we reach zeros > k, after that move the window statically until the number of zeros <= k, at that point you may start increasing the window again.

```java
public int longestOnes(int[] nums, int k){
	int left = 0, right = 0;
	int maxWindow = 0, zeroCounter = 0;

	while (right < nums.length){
		if (nums[right] == 0){
			zeroCounter++;
		}

		// If the zeros > k than until the left pointer reaches a 0
		// it will continue to move the window statically by increasing left
		// and right on the same passage
		if (zeroCounter > k){
			if (nums[left] == 0){
				zeroCounter--;
			}

			left++;  
		}

		int len = right - left + 1;
		if (len > maxWindow){
			maxWindow = len;
		}

		// Note that we're not necessarily increasing the window size
		// if zeroCounter > k => left++
		// so this right++ will keep the window the same size
		right++;  
	}

	return maxWindow;
}
```
<u>TIME COMPLEXITY</u>:  O(N)
<u>SPACE COMPLEXITY</u>: O(1)

---
### Fruit Into Baskets

**Input:** fruits = [1,2,3,2,2]
**Output:** 4
**Explanation:** We can pick from trees [2,3,2,2].
If we had started at the first tree, we would only pick from trees [1,2].

**-> Max length subarray with at most 2 types of numbers (fruits).**

INTUITION: Store the tree type and frequency (number of times it appeared throughout the array fruits) on every passage, check if the hash map size > 2, if so it means there is three types of trees in the window and that is not possible. So now we need to move the left pointer until the map size <= 2, so while the map size is > 2 we are going to take the tree on the left pointer and reduce the frequency by 1, after that we check if the tree type is completely gone from the map (= 0) and then move the left pointer, repeat this until the map size is <= 2 and then you can progress with the right pointer increasing the window.

```java
public int totalFruit(int[] fruits){
	HashMap<Integer, Integer> treesMap = new HashMap<>();
	int left = 0, right = 0;
	int maxLen = 0;

	while (right < fruits.length){
		int rightTree = fruits[right];

		// Store in the map
		treesMap.put(rightTree, treesMap.getOrDefault(rightTree, 0) + 1);

		while (treesMap.size() > 2){
			int leftTree = fruits[left];

			mapTrees.put(leftTree, mapTrees.get(leftTree) - 1);
			if (mapTrees.get(leftTree) == 0){
				mapTrees.remove(leftTree);
			}

			left++;
		}

		int currentLen = right - left + 1;
		if (currentLen > maxLen){
			maxLen = current;
		}

		right++;
	}

	return maxLen;
}
```
<u>TIME COMPLEXITY</u>:  O(2N)
<u>SPACE COMPLEXITY</u>: O(1)

> We can also apply the same approach of after reaching map size > 2, we keep the window that specific size and move it until the map size <= 2, when that happens we may start increasing again. It's a slightly better approach.

---
### Binary Subarrays With Sum
Given a binary array `nums` and an integer `goal`, return _the number of non-empty **subarrays** with a sum_ `goal`.

**Input:** nums = [1,0,1,0,1], goal = 2
**Output:** 4
**Explanation:** The 4 subarrays are bolded and underlined below:
**1,0,1**,0,1
**1,0,1,0**,1
1,**0,1,0,1**
1,0,**1,0,1**

Valid Subarrays => 101, 1010, 0101, 101

INTUITION: 

```java
public int numSubarraysWithSum(int[] nums, int goal){
	if (goal < 0){ return 0; }

	int left = 0, right = 0;
	int windowSum = 0, count = 0;

	while (right < nums.length){
		windowSum += nums[right];
		
		while (windowSum > goal){
			windowSum -= nums[left];
			left++;
		}

		count += left - right + 1;
		right++;
	}
	
	return count;
}
```
<u>TIME COMPLEXITY</u>:  O(4N)
<u>SPACE COMPLEXITY</u>: O(1)

> NOTE: This pattern is relevant for exercises involving counting subarrays with exact matches (count goal - goal-1).

> NOTE: This exercise is the same as **Count Subarray Sum Equals K** and on that exercise we used a hash map to store the prefix sum and the frequency of it happening, but in this case we can optimise it to use a sliding window because its a binary array.

---

