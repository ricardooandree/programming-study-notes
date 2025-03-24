<font color="#7f7f7f"><em>Friday, 01-11-2024 - 15:28</em></font>
#programming #dsa 

This chapter focus on array exercises, traversing, insertion, deletion, searching, etc.

###### DSA exercises solving method:
- Understand the problem (look for all possible outcomes/scenarios) and identify key data structures.
- Consider edge cases and look for patterns.
- Break down the problem and choose the right approach.
- Analyse time and space complexity

<u>NOTE</u>: Always draw the problem on paper.
[[DSA Exercises Methodology]]

### IMPORTANT EXERCISES LIST

| #   | Exercises                                                                  | Notes                                                                         | Diff   |
| --- | -------------------------------------------------------------------------- | ----------------------------------------------------------------------------- | ------ |
| !   | [[#Second Largest Element]]                                                | Basic beginner logic                                                          | Easy   |
| !!  | [[#Remove Duplicates (Sorted Array)]]                                      | Simple two-pointer application                                                | Easy   |
| !   | [[#Left Rotate an array by D places]]                                      | How to circle an array                                                        | Easy   |
| !!! | [[#Move Zeros to End]]                                                     | Simple two-pointer application with a little twist                            | Easy   |
| !!! | [[#Union of Two Sorted Arrays]]                                            | HashMap/Set (if order doesn't matter) - MergeSort approach (if order matters) | Easy   |
| !!! | [[#Find missing number in an unsorted array]]                              | Summation of natural numbers/Array hashing/XOR operation                      | Easy   |
| !!  | [[#Maximum Consecutive Ones]]                                              | Simple counting application (no need for two-pointer)                         | Easy   |
| !!! | [[#Longest subarray with given sum K (positives & positives + negatives)]] | Positive => Simple two-pointer Positive + Negative => Prefix sum              | Easy   |
| !!! | [[#Two Sum]]                                                               | Good to understand hashing and two-pointer                                    | Medium |
| !!  | [[#Sort an array of 0-1-2s]]                                               | Simple counter or Dutch national flag algorithm (3-pointer)                   | Medium |
| !!  | [[#Majority Element (>n/2 times) - Moore's Voting Algorithm]]              | Hashing approach and Moore's voting algorithm (possible candidate)            | Medium |
| !!! | [[#Maximum Subarray Sum - Kadane's Algorithm]]                             | Kadane's algorithm (if sum reaches 0 or below => reset)                       | Medium |
| !   | [[#Stocks Buy and Sell]]                                                   | Simple logic                                                                  | Medium |
| !   | [[#Rearrange Array Elements in Alternating Sign (Positive/Negative)]]      | Simple two-pointer tracker to assign the pos (2n)/neg (2n+1) elements         | Medium |
| !   | [[#Leader in an Array]]                                                    | Simple logic                                                                  | Medium |
| !!! | [[#Count Subarrays with given sum k]]                                      | Prefix Sum Hash                                                               | Medium |
| !!! | [[#Find the maximum sum of a subarray of size K]]                          | Fixed sliding window                                                          | Medium |
| !!! | [[#Smallest subarray with sum >= K]]                                       | Dynamic sliding window                                                        | Medium |
|     |                                                                            |                                                                               | Medium |
|     |                                                                            |                                                                               | Medium |
|     |                                                                            |                                                                               | Medium |
|     |                                                                            |                                                                               |        |

---
---
## Easy
### Largest Element
Loop through the array if the current position value is higher than the maximum value then update the maximum value.
```java
public static int largestElement(int[] array){
	int max = array[0];

	for (int i = 1; i < array.length; i++){
		if (array[i] > max){
			max = array[i];
		}
	}
	return max;
}
```
<u>TIME COMPLEXITY</u>:  O(N)
<u>SPACE COMPLEXITY</u>: O(1)

---
### Second Largest Element
Loop through the array if the current position value is higher than the maximum value then update second maximum value and the maximum value. Also, if the current position is not higher than the maximum value but it's higher than the second maximum value then update the latter.
```java
public static int secondLargestElement(int[] array){
	if (array == null || array.length < 2){
		throw new IllegalArgumentException("Array should have at least two elements.");
	}
	
	int firstMax = array[0];
	int secondMax = Integer.MIN_VALUE;

	for (int i = 0; i < array.length; i++){
		if (array[i] > firstMax){
			secondMax = firstMax;
			firstMax = array[i];
		}
		else if (array[i] < firstMax && array[i] > secondMax){
			secondMax = array[i];
		}
	}
	return secondMax;
}
```
<u>TIME COMPLEXITY</u>:  O(N)
<u>SPACE COMPLEXITY</u>: O(1)

---
### Is Array Sorted?
Check if the current position value is lower than the past one, if so, it's not sorted. Start at position one.
```java
public static boolean isSorted(int[] array){
	for (int i = 1; i < array.length; i++){
		if (array[i] < array[i-1]){
			return false;
		}
	}
	return true;
}
```
<u>TIME COMPLEXITY</u>:  O(N)
<u>SPACE COMPLEXITY</u>: O(1)

---
### Remove Duplicates (Sorted Array)
```java
public static void removeDuplicatesSorted(int[] array){
	int k = 0;

	for (int i = 0; i < array.length; i++){
		if (array[i] != array[k]){
			k++;
			array[k] = array[i];
		}
	}
	return k + 1;
}
```
<u>TIME COMPLEXITY</u>:  O(N)
<u>SPACE COMPLEXITY</u>: O(1)

---
### Left Rotate an array by One place
Save the first element to put on the last one of the rotated array and then shift the elements one by one.
```java
public static void shiftLeftByOne(int[] array){
	int first = array[0];

	for (int i = 0; i < array.length - 1; i++){
		array[i] = array[i+1];
	}
	
	array[array.length - 1] = first;
} 
```
<u>TIME COMPLEXITY</u>:  O(N)
<u>SPACE COMPLEXITY</u>: O(1)

---
### Left Rotate an array by D places
NOTE: Since its left-shift, we start at position k and put it in position i, if it is right shift we need to start at the end and go get the length - k - i - 1 element to put at the last position.

##### 1. Left-Shift (Rotate)
```java
public static void shitLeftByK(int[] array, int k){
	if (array == null || array.length <= 1 || k == 0){ return; }
	
	k = k % array.length;
	if (k == 0){ return; }
	
	int[] aux = new int[k];
	for (int i = 0; i < k; i++){  // save the first k elements
		aux[i] = array[i];
	}

	for (int i = 0; i < array.length - n; i++){ // move the elements to the start
		array[i] = array[i+k];
	}

	for (int i = 0; i < k; i++){  // place elements at the end
		array[array.length - k + i] = aux[i];
	}
}
```
<u>TIME COMPLEXITY</u>:  O(N)
<u>SPACE COMPLEXITY</u>: O(k)

##### 2. Right-Shift (Rotate)
```java
public static void shitRightByK(int[] array, int k){
	if (array == null || array.length <= 1 || k == 0){ return; }
	
	k = k % array.length;
	if (k == 0){ return; }
	
	int[] aux = new int[k];
	for (int i = 0; i < k; i++){  // save the last k elements 
		aux[i] = array[array.length - k + i];
	}

	for (int i = 0; i < array.length - k; i++){  // move the elements to the end
		array[array.length - i - 1] = array[array.length - k - i - 1];
	}

	for (int i = 0; i < k; i++){  // place the elements at the start
		array[i] = aux[i];
	}
}
```
<u>TIME COMPLEXITY</u>:  O(N)
<u>SPACE COMPLEXITY</u>: O(k)

---
### Move Zeros to End
**Brute force:** Run through the array take what's not zero position by position and count the zeros or array.length - aux.length after that substitute de original array.

##### 1. Two-Pointer Method with 2 loops
```java
public static void moveZerosToEnd(int[] array){
	int j = 0;
	while (j < array.length && array[j] != 0){
		j++;
	}

	if (j == array.length){
		return;
	}
	
	for (int i = j + 1; i < array.length; i++){
		if (array[i] != 0){
			array[j] = array[i];
			array[i] = 0;
			j++;
		}
	}
}
```
<u>TIME COMPLEXITY</u>:  O(N)
<u>SPACE COMPLEXITY</u>: O(1)

##### 2. Two-Pointer Method with 1 loop
```java
public static void moveZerosToEnd(int[] array){
	int left = 0, right = 0;
	
	while (right < array.length){
		if (array[right] != 0){   
			int temp = array[left];      // temp is needed in case array: [1]
			array[left] = array[right];  // so it doesnt become [0]
			array[right] = temp;
			left++;
		}

		right++;
	}
}
```
<u>TIME COMPLEXITY</u>:  O(N)
<u>SPACE COMPLEXITY</u>: O(1)

---
### Linear Search
```java
public static int linearSearch(int[] array, int n){
	for (int i = 0; i < array.length; i++){
		if (array[i] == n){
			return i;
		}
	}
	return -1;
}
```
<u>TIME COMPLEXITY</u>:  O(N)
<u>SPACE COMPLEXITY</u>: O(1)

---
### Union of Two Sorted Arrays
##### 1. Map
Use a map to keep up with the frequencies of a certain value within the arrays, by the end, create a new array with the values of the keys of the hash map.

NOTE: Map keys are unique and ordered automatically.
```java
public static int[] union(int[] array1, int[] array2){
	int size1 = array1.length;
	int size2 = array2.length;
	
	HashMap<Integer, Integer> map = new HashMap<>();
	
	// Store frequencies for array1
	for (int i = 0; i < size1; i++){
		map.put(array1[i], 1);
	}

	// Store frequencies for array2
	for (int i = 0; i < size2; i++){
		map.put(array2[i], 1);
	}

	int i = 0;
	int[] union = new int[map.size()];
	
	for (int val : map.keySet()){
		array[i] = val;
		i++;
	}

	return union;
}
```
<u>TIME COMPLEXITY</u>:  O(N LOG N) - highest array size
<u>SPACE COMPLEXITY</u>: O(N) - highest array size

##### 2. Set
Use a set to store the values in a unique manner and transfer them to a new array at the end.

NOTE: Set values are ordered automatically.
```java
public static int[] union(int[] array1, int[] array2){
	int size1 = array1.length;
	int size2 = array2.length;

	HashSet<Integer> set = new HashSet<>();

	// Store frequencies for array1
	for (int num : array1){ set.add(num); }

	// Store frequencies for array2
	for (int num : array2){ set.add(num); }

	int i = 0;
	int[] union = new int[set.size()];
	for (int val : set){
		union[i] = val; 
	}

	return union;
}
```
<u>TIME COMPLEXITY</u>:  O(N LOG N) 
<u>SPACE COMPLEXITY</u>: O(N) - highest array size

##### 3. Two-Pointer (Optimal)
Similarly to merge sort we can use 2 pointers for each partition arrays and run through them and do the needed comparisons.
```java
public static void union(int[] array1, int[] array2){
	int size1 = array1.length;
	int size2 = array2.length;
	int[] union = new int[size1 + size2];
	int i = 0, j = 0, k = 0;
	int value = -1;

	while (i < size1 && j < size2){
		if (array1[i] <= array2[j]){
			value = array1[i];
			i++;
		} else {
			value = array2[j];
			j++;
		}

		// Check if it's not a duplicate
		if (k == 0){  // First value case
			union[k] = value;
			k++;
		}
		else if (union[k - 1] != value){
			union[k] = value;
			k++;
		}
	}

	// Add the rest of the missing array1 
	while (i < size1){
		if (union[k - 1] != array1[i]){
			union[k] = array1[i];
			k++;
		}
		i++;
	}

	// Add the rest of the missing array2
	while (j < size2){
		if (union[k - 1] != array2[j]){
			union[k] = array2[j];
			k++;
		}
		j++;
	}
	
	return union;
}
```
<u>TIME COMPLEXITY</u>:  O(N) 
<u>SPACE COMPLEXITY</u>: O(N+M)

---
### Find missing number in an unsorted array
##### 1. Hash Array/Map/Set
```java
public static int missingNumber(int[] array, int n){
	int[] hash = new int[n + 1];

	for (int i = 0; i < array.length; i++){
		hash[array[i]] = 1;
	}

	for (int i = 1; i < hash.length; i++){
		if (hash[i] == 0){
			return i;
		}
	}

	return -1;
}
```
<u>TIME COMPLEXITY</u>:  O(N) 
<u>SPACE COMPLEXITY</u>: O(N)

##### 2. Summation of Natural Numbers
It's important to know the equation that gives the sum of N natural numbers. This isn't the optimal solution because if n is too big it overflows the integer.
```java
public static int missingNumber(int[] array, int n){
	int totalSum = (n * (n + 1)) / 2;
	int arraySum = 0;
	
	for (int i = 0; i < array.length; i++){
		arraySum += array[i];
	}

	return totalSum - arraySum;
}
```
<u>TIME COMPLEXITY</u>:  O(N) 
<u>SPACE COMPLEXITY</u>: O(1)

##### 3. XOR Operation - Optimal
XOR operation between two equal numbers is 0, that way you can identify the missing number.
```java
public static int missingNumber(int[] array, int n){
	int xor1 = 0, xor2 = 0;

	for (int i = 0; i < array.length; i++){
		xor1 = xor1 ^ (i + 1);
		xor2 = xor2 ^ array[i];
	}

	xor1 = xor1 ^ n;
	return xor1 ^ xor2;
}
```
<u>TIME COMPLEXITY</u>:  O(N) 
<u>SPACE COMPLEXITY</u>: O(1)

---
### Maximum Consecutive Ones
```java
public static int maxConsecutiveOnes(int[] array){
	int max = 0, count = 0;
	for (int i = 0; i < array.length; i++){
		if (array[i] == 0){
			count = 0;
		}
		else {
			count++;
		}

		if (count > max){ max = count; }
	}

	return max;
}
```
<u>TIME COMPLEXITY</u>:  O(N) 
<u>SPACE COMPLEXITY</u>: O(1)

---
### Find the number that appears once (others appear twice)
##### 1. Hashing - Array/Map/Set
NOTE: Doesn't work for negative numbers
```java
public static int singleElement(int[] array){
	HashMap<Integer,Integer> map = new HashMap<>();

	for (int i = 0; i < array.length; i++){
		map.put(array[i], map.getOrDefault(array[i], 0) + 1);
	}
	
	for (Map.Entry<Integer, Integer> entry : map.entrySet()){
		if (entry.getValue() == 1){
			return entry.getKey();
		}
	}
	return -1;
}
```
<u>TIME COMPLEXITY</u>:  O(2N) 
<u>SPACE COMPLEXITY</u>: O(N)

##### 2. XOR Operation
```java
public static int singleElement(int[] array){
	int xor = 0;

	for (int num : array){
		xor = xor ^ num;
	}

	return xor;
}
```
<u>TIME COMPLEXITY</u>:  O(N) 
<u>SPACE COMPLEXITY</u>: O(1)

---
### Longest subarray with given sum K (positives & positives + negatives)
```java
public int longestSubarrayWithSumK(int[] array, int k){
	int j = 0, sum = 0, maxLen = 0;

	for (int i = 0; i < array.length; i++){
		// OR if (i != 0 && sum == 0) { j = i; }
		sum += array[i];

		if (sum == k){
			if (i + 1 - j > maxLen){  // Size of the largest window/subarray
				maxLen = i + 1 - j;
			}

			if (i + 1 < array.length){ j = i + 1; }
			sum = 0;
		}
	}
	return maxLen;
}
```
<u>TIME COMPLEXITY</u>:  O(N) 
<u>SPACE COMPLEXITY</u>: O(1)

#### Hashing/Prefix Sum - Works with positives + negatives
```java
public int longestSubarrayWithSumK(int[] array, int k){
	HashMap<Integer, Integer> map = new HashMap<>();
	int prefixSum = 0, maxLen = 0;

	for (int i = 0; i < array.length; i++){
		prefixSum += array[i];

		if (prefixSum == k){
			maxLen = i + 1;
		}
		if (map.contains(prefixSum - k)){
			possibleMaxLen = i - map.get(prefixSum - k);

			if (possibleMaxLen > maxLen){ maxLen = possibleMaxLen; }
		}

		// If required to work with negative numbers. For positives just directly store the prefixSum in the hash map
		if (!map.contains(prefixSum)){ map.put(prefixSum, i); }
	}
	return maxLen;
}
```
<u>TIME COMPLEXITY</u>:  O(N) - map contains/get/put methods are on average O(1)
<u>SPACE COMPLEXITY</u>: O(N)

---
---
## Medium
### Two Sum
##### 1. Hashing
Stores in a hash the current position value of the array and the index as a key-value pair.
```java
public static int[] twoSum(int[] array, int target){
	HashMap<Integer, Integer> map = HashMap<>();

	for (int i = 0; i < array.length; i++){
		if (map.containsKey(target - array[i])){
			return new int[]{map.get(target - array[i]), i};
		}

		hash.put(array[i], i);
	}
}
```
<u>TIME COMPLEXITY</u>:  O(N) or O(N^2) - HashMap collisions worst case
<u>SPACE COMPLEXITY</u>: O(N)
##### 2. Two-Pointer - Wrong Implementation X 
The array needs to be sorted before applying the two-pointer method. And we need a hash map to track the original array indexes. 

**NOTE:** Doesn't work if the list has repeated numbers since it will override the map entries. Use a two-dimensional array instead.
```java
public static int[] twoSum(int[] array, int target){
	HashMap<Integer, Integer> map = new HashMap<>();
	for (int i = 0; i < array.length; i++){
		map.put(array[i], i);
	}

	quickSort(array, 0, array.length - 1);

	int i = 0, j = 0;
	while (left < right){
		int sum = array[left] + array[right];
		
		if (sum == target){
			int indexOne = map.get(array[left]);
			int indexTwo = map.get(array[right]);
			return new int[]{indexOne, indexTwo};
		}

		if (sum < target){ left++; }
		else { right--; }
	}
}
```
<u>TIME COMPLEXITY</u>: O(N LOG N) or O(N^2) - HashMap collisions worst case
<u>SPACE COMPLEXITY</u>: O(N)

---
### Sort an array of 0-1-2s
##### 1. Counters
Count the number of 0-1-2s and override the array with the ordered elements.
```java
public static void sortColors(int[] array){
	int zeros = 0, ones = 0, twos = 0;

	for (int i = 0; i < array.length; i++){
		if (array[i] == 0){ zeros++; }
		else if (array[i] == 1){ ones++; }
		else { twos++; }
	}

	for (int i = 0; i < zeros; i++){ array[i] = 0; }
	for (int i = zeros; i < zeros + ones; i++){ array[i] = 1; }
	for (int i = zeros + ones; i < array.length; i++){ array[i] = 2; }
}

```
<u>TIME COMPLEXITY</u>:  O(3N)
<u>SPACE COMPLEXITY</u>: O(1)

##### 2. Dutch National Flag Algorithm
Use 3 pointers to keep track of the elements positioning.
```java
public static void sortColors(int[] array){
	int low = 0, mid = 0, high = array.length - 1;

	while (mid <= high){
		if (array[mid] == 0){
			int aux = array[mid];
			array[mid] = array[low];
			array[low] = aux;
			low++;
			mid++;
		}
		else if (array[mid] == 1){
			mid++;
		}
		else if (array[mid] == 2){
			array[mid] = array[high];
			array[high] = 2;
			high--;
		}
	}
}
```
<u>TIME COMPLEXITY</u>:  O(N)
<u>SPACE COMPLEXITY</u>: O(1)

---
### Majority Element (>n/2 times) - Moore's Voting Algorithm

##### 1. Hashing
```java
public int majorityElement(int[] array){
	HashMap<Integer,Integer> map = new HashMap<>();

	for (int i = 0; i < array.length; i++){
		map.put(array[i], map.getOrDefault(array[i], 0) + 1);
	}

	for (Map.Entry<Integer, Integer> entry : map.entrySet()){
		if (entry.GetValue() > (array.length / 2)){
			return entry.GetKey();
		}
	}
	return -1;
}
```
<u>TIME COMPLEXITY</u>:  O(N)
<u>SPACE COMPLEXITY</u>: O(N)

##### 2. Moore's Voting Algorithm
The first part consists of selecting a candidate, we elect a first candidate and then count how many times it shows up through iterations. If the count reaches 0, it means in that section of the array (since it first appeared to when it reached count = 0) there's no real candidate. So assign a new candidate (the next element) and do the same. By the end, we'll have a candidate which hasn't been "annulled" so its a possible candidate. Then we verify if it's the real candidate by counting through the array.
```java
public int majorityElement(int[] array){
	int count = 0, elem = array[0];
	
	for (int i = 0; i < array.length; i++){
		if (array[i] == elem){ count++; }
		else { count--; }

		if (count == 0 && i + 1 < array.length){
			elem = array[i+1];
		}
	}

	count = 0;
	for (int num : array){
		if (num == elem){
			count++;
		}
	}

	if (count > array.length / 2){ return elem; }
	return -1;
}
```
<u>TIME COMPLEXITY</u>:  O(N)
<u>SPACE COMPLEXITY</u>: O(1)

##### 3. EXTRA - Brute Force (Practice)
```java
public int majorityElement(int[] array){
	for (int i = 0; i < array.length; i++){
		int count = 0;

		for (int j = 0; j < array.length; j++){
			if (array[j] == array[i]){ count++; }
		}

		if (count > (array.length / 2)){ return array[i]; }
	}
	return -1;
}
```
<u>TIME COMPLEXITY</u>:  O(N^2)
<u>SPACE COMPLEXITY</u>: O(1)

---
### Maximum Subarray Sum - Kadane's Algorithm

##### 1. Prefix Sum - O(N^2)
```java
public int maxSubArray(int[] array){
	int sum = 0, max = Integer.MIN_VALUE;

	for (int i = 0; i < array.length; i++){
		for (int j = i; i < array.length; j++){
			sum += array[j];

			if (sum > max){ max = sum; }
		}
	}
	return max;
}
```

##### 2. Kadane's Algorithm
The Kadane's algorithm focus on iterating through the array once and discarding the negative sums, because, if the current subarray sum is negative, continuining with this subarray will only reduce the overall possible maximum sum. So, we start the subarray from the next element instead.

**NOTE:** Only works if the array has 1 positive number or else it will return 0.
```java
public int maxSubArray(int[] array){
	int sum = array[0], max = Integer.MIN_VALUE;

	for (int i = 0; i < array.length; i++){
		sum += array[i];
		
		if (sum > max){ max = sum; }
		if (sum < 0){ sum = 0; }
	}

	// Consider the sum of the empty array
	if (max < 0){ max = 0; }
	return max;

	// IF WE WANT TO FIND START & END INDEXES
	for (int i = 0; i < array.length; i++){
		if (sum == 0){ start = i; }
		sum += array[i];
		
		if (sum > max){
			max = sum;
			startIndex = start;
			endIndex = i;
		}
		if (sum < 0){ sum = 0; }
	}

	// INCLUDING NEGATIVE ONLY ARRAYS:
	int sum = nums[0], max = nums[0];
	for (int i = 1; i < nums.length; i++){
		if (sum + nums[i] > sum){
			sum += nums[i];
		}
		else {
			sum = nums[i];
		}

		if (sum > max){ max = sum; }
	}

	return max;
}
```
<u>TIME COMPLEXITY</u>:  O(N)
<u>SPACE COMPLEXITY</u>: O(1)

---
### Stocks Buy and Sell
Keep track of the minimum price and the maximum profit will be the sell day where current iteration price - minimum price is the highest.
```java
public int stockBuySell(int[] prices) {
	int min = prices[0], maxProfit = 0;

	for (int i = 1; i < prices.length; i++){
		if (prices[i] < min){
			min = prices[i];
		}

		if (prices[i] - min > maxProfit){
			maxProfit = prices[i] - min;
		}
	}
	return maxProfit;
}
```
<u>TIME COMPLEXITY</u>:  O(N)
<u>SPACE COMPLEXITY</u>: O(1)

---
### Rearrange Array Elements in Alternating Sign (Positive/Negative)
##### Brute Force 
Consists of running through the array, store in two auxiliar arrays the positives and negatives separately and then rearrange the original array.
```java
public static int[] RearrangebySign(int[] array){
	int[] pos = new int[array.length / 2];
	int[] neg = new int[array.length / 2];
	int posIndex = 0, negIndex = 0;
	
	for (int i = 0; i < array.length; i++){
		if (array[i] > 0){
			pos[posIndex] = array[i];
			posIndex++;
		}
		else {
			neg[negIndex] = array[i];
			negIndex++;
		}
	}

	for (int i = 0; i < array.length / 2; i++){
		array[2*i] = pos[i];
		array[2*i+1] = neg[i];
	}

	return array;
}
```
<u>TIME COMPLEXITY</u>:  O(N+N/2)
<u>SPACE COMPLEXITY</u>: O(N)

##### Negative/Positive Pointers Tracking
```java
public static int[] RearrangebySign(int[] array){
	int[] aux = new int[array.length];
	int pos = 0, neg = 1;

	for (int i = 0; i < array.length; i++){
		if (array[i] > 0){
			aux[pos] = array[i];
			pos += 2;
		}
		else {
			aux[neg] = array[i];
			neg += 2;
		}
	}

	return aux;
}
```
<u>TIME COMPLEXITY</u>:  O(N)
<u>SPACE COMPLEXITY</u>: O(N)

### Rearrange Array Elements in Alternating Sign (Positive/Negative) II
In this one there's no guarantee that the array has the same amount of positive and negative elements.
If any of the positive and negative numbers are left, add them at the end without altering the order.
In this case we still store the elements in the auxiliar positive and negative arrays, and then replace the original array up until the elements can be alternated. Once it reaches that point and the next elements won't be alternated, we replace the leftovers.
```java
public static int[] RearrangebySignII(int[] array){
	int[] neg = new int[array.length];
	int[] pos = new int[array.length];
	int posIndex = 0, negIndex = 0;
	
	for (int i = 0; i < array.length; i++){
		if (array[i] > 0){
			pos[posIndex] = array[i];
			posIndex++;
		}
		else {
			neg[negIndex] = array[i];
			negIndex++;
		}
	}

	if (pos.length > neg.length){
		for (int i = 0; i < neg.length; i++){
			array[2*i] = pos[i];
			array[2*i+1] = neg[i];
		}

		for (int i = neg.length; i < pos.length; i++){
			array[neg.length + i] = pos[i];
		}
	} else {
		for (int i = 0; i < pos.length; i++){
			array[2*i] = pos[i];
			array[2*i+1] = neg[i];
		}

		for (int i = pos.length; i < neg.length; i++){
			array[pos.length + i] = neg[i];
		}
	}
	return array;
}
```
<u>TIME COMPLEXITY</u>:  O(2N)
<u>SPACE COMPLEXITY</u>: O(N)

---
### Next Permutation
«A permutation refers to a rearrangement of digits in a number. For example, the permutations of the array `[1, 2, 3]` are:
- `[1, 2, 3]`
- `[1, 3, 2]`
- `[2, 1, 3]`
- `[2, 3, 1]`
- `[3, 1, 2]`
- `[3, 2, 1]`

##### Step 1: Identify the Longest Non-Decreasing Suffix

For `[2, 1, 5, 4, 3, 0, 0]`, we want to find the next permutation that is just slightly larger than the current one. To do so, we work backwards through the array to find the "breakpoint" where `a[i] < a[i + 1]`.

For example, we begin by examining permutations of the last few digits in the array (starting from `(0, 0)` and progressing). Eventually, we see that the <u>smallest larger</u> number is achieved when the sequence is rearranged as `(2, 3, 0, 0, 1, 4, 5)`.

To pinpoint the breakpoint:

**Array: (2, <u>1</u>, 5, 4, 3, 0, 0)**

We find that at index `i = 1`, `a[i] = 1`, and this marks the point where we can make our change.

##### Step 2: Find the Smallest Number Larger than `a[i]`

Once we find the breakpoint (`a[i] = 1` in this case), the next step is to find the <u>smallest number that is larger</u> than `a[i]` from the portion of the array to the right. This ensures that we are creating the **next permutation** and not just any random permutation.

The numbers greater than `1` are `5`, `4`, and `3`.
The smallest of these is `3`.

At this point, we swap `a[i] = 1` with `3`.

**Array: (2, <u>3</u>, 5, 4, <u>1</u>, 0, 0)**

##### Step 3: Sort the Remaining Elements

The final step is to sort the subarray after the swapped index in **ascending order**. Since the numbers after the breakpoint are in descending order by nature, we only need to reverse them to get the smallest arrangement possible.

**Array: (2, 3, 0, 0, 1, 4, 5) = Next Permutation**

```java
public static void nextPermutation(int[] nums) {
	int breakpointIndex = -1;

	// Step 1: Identify the breakpoint
	// We traverse the array from right to left and look for the first pair where nums[i] < nums[i + 1].
	// This index marks the point where the array stops increasing.
	for (int i = nums.length - 2; i >= 0; i--){
		if (array[i] < array[i + 1]){
			breakpointIndex = i;
			break;
		}
	}

	// Step 2: Find the smallest element larger than nums[breakpointIndex]
	if (breakpointIndex == -1){
		// If no such breakpoint is found, the array is in descending order, so we can reverse it to get the smallest permutation.
		// This case handles situations where we already have the largest permutation.
	}

	// NOTE: To find the smallest element larger than nums[breakpointIndex]
	// we do not need to search from the breakpoint to the end of the array.
	// Instead, we can simply loop from the end of the array towards the breakpoint,
	// and take the first element that is larger than nums[breakpointIndex].
	// This is efficient because if any element larger than nums[breakpointIndex]
	// existed after the first one we find, that would have been the new breakpoint.
	// Example: [2 1 5 4 3 1.5 2] here the breakpoint is 1.5, not 1.
	// Example: [2 1 5 4 3 0 0] the number we are looking for is 3,
	// which is the first number larger than 1.
	for (int i = nums.length - 1; i > breakpointIndex; i--){
		if (nums[i] > nums[breakpointIndex]){
			// Swap the found element with nums[breakpointIndex]
			int tmp = nums[i];
			nums[i] = nums[breakpointIndex];
			nums[breakpointIndex] = tmp;
			break;
		}
	}

	// Step 3: Sort the remaining elements in ascending order
	// After the swap, the subarray after breakpointIndex will be in descending order. 
	// To get the next smallest permutation, we reverse this part to make it ascending.
	int left = breakpointIndex + 1;
	int right = nums.length - 1;
	while (left < right){
		// Swap the elements at left and right
		int tmp = nums[left];
		nums[left] = nums[right];
		nums[right] = tmp;
		left++;
		right--;
	}
}
```
<u>TIME COMPLEXITY</u>:  O(3N)
<u>SPACE COMPLEXITY</u>: O(1)

---
### Leader in an Array
A leader in an array is an element where all the elements at its right are smaller than him.
Traverse through the array from right to left and see if the current element is smaller than the maximum, technically we want to see if the array is sorted in a descending order.
```java
public static int[] leadersInArray(int[] nums){
	int[] aux = new int[nums.length];
	int max = Integer.MIN_VALUE;
	int j = 0;
	
	for (int i = nums.length - 1; i >= 0; i--){
		if (nums[i] > max){ 
			aux[j] = nums[i];  // Store the leader
			max = nums[i];     // Update the maximum
			j++;   
		}
	}

	// Reverse the leaders to maintain correct order
	int[] result = new int[j];
	for (int i = 0; i < j; i++){
		result[i] = aux[j - i - 1];
	}
	
	return result;
}
```
<u>TIME COMPLEXITY</u>:  O(N)
<u>SPACE COMPLEXITY</u>: O(N)

---
### Longest Consecutive Sequence in an Array
##### Sorting 
```java
public static int longestConsecutive(int[] array) {
    if (array.length == 0) { return 0; }  // If array is empty

    Arrays.sort(array);
    int count = 1, len = 1;

    for (int i = 0; i < array.length - 1; i++){
        if (array[i+1] == array[i] + 1){  // If valid sequence
            count++;
        } else if (array[i+1] > array[i] + 1){  // If new sequence start
		    count = 1;
	    } 

		// Else => array[i+1] == array[i] which we do not increase counter or do anything at all

		if (count > len){ len = count; }
    }
    return len;
}
```
<u>TIME COMPLEXITY</u>:  O(N LOG N + N = NLOGN)
<u>SPACE COMPLEXITY</u>: O(1)

##### Hashing - Set
The main idea is to check if the current element has an element - 1 in the set, if it does, it means its not the start of the subsequence. If it doesn't then it means its the start of the sequence so, we start counting the sequence by finding element + 1 in the set until it doesn't exist.

```java
public static int longestConsecutive(int[] array) {
	if (array.length == 0){ return 0; }

	Set<Integer> set = new HashSet<>();
	int len = 0, count = 0;

	for (int i = 0; i < array.length; i++){
		set.add(array[i]);
	}
	
	for (int elem : set){
		if (!set.contains(elem - 1)){  // found the start of the sequence
			count = 1; 
			int nextElem = elem + 1;
			
			while (set.contains(nextElem)){ // iterates through the valid sequence
				nextElem++;
				count++;
			}

			if (count > len){ len = count; }
		}
	}
	return len;
}
```
<u>TIME COMPLEXITY</u>:  O(3N)
<u>SPACE COMPLEXITY</u>: O(N)

---
### Set Matrix Zeros

##### Brute Force Approach
Traverse the matrix and on the row/column where a zero exists, update those row/column elements that are NOT zero to -1.

Then, traverse the matrix again and update all -1 elements to 0.
```java
public static void setMatrixZeros(int[][] matrix){
	int n = matrix.length;
	int m = matrix[n-1].length;

	// Step 1: Turn the row and column elements != 0 where 0 exists into -1
	for (int i = 0; i < n; i++){
		for (int j = 0 ; j < m; j++){
			if (matrix[i][j] == 0){
				makeRow(matrix, i);
				makeCol(j);
			}
		}
	}

	// Step 2: Traverse the matrix and update the elements -1 to 0
	for (int i = 0; i < n; i++){
		for (int j = 0 ; j < m; j++){
			if (matrix[i][j] == -1){
				matrix[i][j] = 0;
			}
		}
	}
}

public static void makeRow(int[][] matrix, int row){
	for (int j = 0; j < matrix.length; j++){
		if (matrix[row][j] != 0){
			matrix[row][j] = -1;
		}
	}
}

public static void makeCol(int[][] matrix, int col){
	for (int i = 0; i < matrix.length; i++){
		if (matrix[i][col] != 0){
			matrix[i][col] = -1;
		}
	}
}
```
<u>TIME COMPLEXITY</u>:  O(NMx(N+M) + (NM)) ~ O(NM^3)
<u>SPACE COMPLEXITY</u>: O(1)

##### Better Approach
Use two hash arrays that reflect the rows and columns where the elements will have to be updated to 0.

Those hash arrays are a row array and a column array.
```java
public static void setMatrixZeros(int[][] matrix){
	int n = matrix.length;
	int m = matrix[n-1].length;
	int[] row = new int[n];
	int[] col = new int[m];

	// Step 1: Traverse the matrix and hash on the row/col arrays the row/cols that will have a zero
	for (int i = 0; i < n; i++){
		for (int j = 0; j < m; j++){
			if (matrix[i][j] == 0){
				row[i] = 1;
				col[j] = 1;
			}
		}
	}

	// Step 2: Traverse the matrix and update the elements based on the hash
	for (int i = 0; i < n; i++){
		for (int j = 0; j < m; j++){
			if (row[i] == 1 || col[j] == 1){
				matrix[i][j] = 0;
			}
		}
	}
}
```
<u>TIME COMPLEXITY</u>:  O(2NM)
<u>SPACE COMPLEXITY</u>: O(N+M)

##### Optimal Approach
The intuition behind this algorithm is to optimize space usage by using the first row and column of the matrix as a "hash table" to record where zeros are located, instead of using an additional data structure like a separate array. 

The idea is simple: when a zero is encountered, mark its presence in the corresponding row and column by setting the first element of that row and column to zero. Additionally, use a separate variable `col0` to track whether the first column should be zeroed out (since the first column overlaps with the rest of the matrix's zero tracking). 

Once this marking is complete, you traverse the matrix again, using the "hash" in the first row and column to zero out the appropriate cells. 

Finally, you update the first row and first column based on their respective markers, ensuring the entire matrix is correctly updated with minimal extra space usage.
```java
public static void setMatrixZeros(int[][] matrix) {
	// row hash -> matrix[n][0] - first column
	// col hash -> matrix[0][m] - first row
	int n = matrix.length;
	int m = matrix[n-1].length;
	int col0 = 1;

	// Step 1: Traverse the matrix and hash the zeros into the first row/col
	for (int i = 0; i < n; i++){
		for (int j = 0; j < m; j++){
			if (matrix[i][j] == 0){
				matrix[i][0] = 0;  // Hash/mark row

				if (j != 0){       // Hash/mark col
					matrix[0][j] = 0;
				}
				else { col0 = 0; }
			}
		}
	}

	// Step 2: Traverse the "inside" of the matrix and update the elements based on the hashes
	for (int i = 1; i < n; i++){
		for (int j = 1; j < m; j++){
			if (matrix[0][j] == 0 || matrix[i][0] == 0){
				matrix[i][j] = 0;
			}
		}
	}

	// Step 3: Update the first row and then the first column
	if (matrix[0][0] == 0){
		for (int j = 0; j < m; j++){
			matrix[0][j] = 0;
		}
	}

	if (col0 == 0){
		for (int i = 0; i < n; i++){
			matrix[i][0] = 0;
		}
	}
}
```
<u>TIME COMPLEXITY</u>:  O(2NM)
<u>SPACE COMPLEXITY</u>: O(N)

---
### Rotate Matrix by 90 Degrees/Rotate Image

##### Brute Force Approach
Create a new correct (rotated) matrix

Note the following:
1 2 3      7 4 1
4 5 6  ->  8 5 2
7 8 9      9 6 3 

(0)(0) -> (0)(2)   (1)(0) -> (0)(1)   (2)(0) -> (0)(0)
(0)(1) -> (1)(2)   (1)(1) -> (1)(1)   (2)(1) -> (1)(0)
(0)(2) -> (2)(2)   (1)(2) -> (2)(1)   (2)(2) -> (2)(0)

```java
public static int[][] rotateMatrix(int[][] matrix){
	int n = matrix.length;
	int[][] aux = new int[n][n];

	for (int i = 0; i < n; i++){
		for (int j = 0; j < n; j++){
			aux[j][n-1-i] = matrix[i][j];
		}
	}
	return aux;
}
```
<u>TIME COMPLEXITY</u>:  O(NM)
<u>SPACE COMPLEXITY</u>: O(NM)


##### Optimal Approach - In Place 
Step 1: Transpose the matrix (rows become columns)
Step 2: Reverse the rows
```java
public static int[][] rotateMatrix(int[][] matrix){
	int n = matrix.length;
	
	// Step 1 - Transpose the matrix
	for (int i = 0; i < n; i++){
		for (int j = i + 1; j < n; j++){  // j = i + 1, only swap one diagonal of the matrix, if we swap both it will stay the same
			int aux = matrix[i][j];
			matrix[i][j] = matrix[j][i];
			matrix[j][i] = aux;
		}
	}

	// Step 2 - Reverse the rows
	for (int i = 0; i < n; i++){
		for (int j = 0; j < n / 2; j++){  // only reverse half of a row, or else it will do it twice and stay the same
			int aux = matrix[i][j];
			matrix[i][j] = matrix[i][n-1-j];
			matrix[i][n-1-j] = aux;
		}
	}
}
```
<u>TIME COMPLEXITY</u>:  O(NM)
<u>SPACE COMPLEXITY</u>: O(1)

---
### Spiral Matrix
The intuition behind this algorithm is to have 4 pointers that mark the start and end of each loop, those being the top, bottom, left, and right. 

Additionally we need to check for the edge-cases where the matrix is a row or a column.
```java
public static void spiralMatrix(int[][] matrix){
	int top = 0, bottom = matrix.length - 1;
	int left = 0, right = matrix[0].length - 1;
	
	while(left <= right && top <= bottom ){
		// Print left to right
		for (int i = left; i <= right; i++){
			System.out.print(matrix[top][i]);
		}
		top++;
		
		// Print top to bottom
		for (int i = top; i <= bottom; i++){
			System.out.print(matrix[i][right]);
		}
		right--;
		
		// Print right to left
		if (top <= bottom){
			for (int i = right; i >= left; i--){
				System.out.print(matrix[bottom][i]);
			}
			bottom--;
		}
	
		// Print bottom to top
		if (left <= right){
			for (int i = bottom; i >= top; i--){
				System.out.print(matrix[i][left]);
			}
			left++;
		}
	}
}
```
<u>TIME COMPLEXITY</u>:  O(NM)
<u>SPACE COMPLEXITY</u>: O(1)

---
### Count Subarrays with given sum k
Instead of conventionally store the prefix sum and index in the hash map, store the prefix sum and the number of times that sum has appeared that way at the current sum you'll be able to know how many subarrays of a set value of prefix sum appeared up until that point.

```java
public int subarraySum(int[] nums, int k) {
	HashMap<Integer, Integer> map = new HashMap<>();
	int prefixSum = 0, count = 0;

	for (int i = 0; i < nums.length; i++){
		prefixSum += nums[i];

		// Tracks the first valid subarrays and ones like [3 3 -3] 
		if (prefixSum == k){  
			count++;
		}

		// Get the amount of times prefixSum - k appeared
		if (map.containsKey(prefixSum - k)){
			count += map.get(prefixSum - k);
		}

		// If that prefix sum already appeared once update the counter of the map
		if (map.containsKey(prefixSum)){
			map.put(prefixSum, map.getOrDefault(prefixSum, 0) + 1);
		}
		else {
			map.put(prefixSum, 1);
		}
	}

	return count;
}
```
<u>TIME COMPLEXITY</u>:  O(N)
<u>SPACE COMPLEXITY</u>: O(N)

---
### Find the maximum sum of a subarray of size K 
Fixed sliding window application

Sliding Window Approach
1. Use a fixed window of size `k`.
2. Compute the sum of the first `k` elements (initial window).
3. Slide the window one step at a time:
    - **Remove** the leftmost element.
    - **Add** the next element from the right.
    - **Update the maximum sum.**
4. **Repeat until the end of the array is reached.**

```java
public static int maxSumSubarray(int[] array, int k){
	if (array.length < k){ return -1; }

	int maxSum = 0, windowSum = 0;
	for (int i = 0; i < k; i++){  // Starts window until its of size k
		windowSum += array[i];
	}

	maxSum = windowSum;

	for (int i = k; i < array.length; i++){
		windowSum += array[i] - array[i-k]; // add new element, remove leftmost element
		if (windowSum > maxSum){ maxSum = windowSum; }
	}

	return maxSum;
}
```
<u>TIME COMPLEXITY</u>:  O(N)
<u>SPACE COMPLEXITY</u>: O(1)

---
### Smallest subarray with sum >= K
Dynamic sliding window application

Dynamic Sliding Window Approach
1. **Expand the window** by adding elements to `windowSum` until `windowSum ≥ k`
2. **Shrink the window** from the left while `windowSum ≥ k` (to minimize subarray size).
3. **Update the minimum length** whenever a valid subarray is found.
4. **Repeat until the end of the array.**

```java
public static int minSubarrayLength(int[] array, int k){
	int minLength = Integer.MAX_VALUE;
	int windowSum = 0;
	int left = 0;  // left pointer of the window

	for (int right = 0; right < array.length; right++){
		windowSum += array[i];

		// Check if window is valid (>=k) and if so check if the size of the
		// current window is smaller than the minLength size
		// then decrease the window by removing the leftmost element 
		if (windowSum >= k){
			int currentLen = right - left + 1;
			
			if (currentLen < minLength){
				minLength = currentLen;
			}
			
			windowSum -= array[left];
			left++;
		}
	}
	return minLength;
}
```
<u>TIME COMPLEXITY</u>:  O(N)
<u>SPACE COMPLEXITY</u>: O(1)