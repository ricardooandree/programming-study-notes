<font color="#7f7f7f"><em>Friday, 01-11-2024 - 15:28</em></font>
#programming #dsa 

This chapter focus on binary search in 1D, 2D arrays and answers.

## Theoretical Fundaments
Binary search only works in a **sorted search space**.

It works by splitting the search space in half and excluding one of the halves and iteratively doing that until we find what we want. 

(3, 4, 6, 7, 9, 12, 16, 17) -> target = 6
low      mid           high

is mid == target? No => Pick the correct half, since target = 6 and mid = 7, target < mid => target is in the first half.

(3, 4, 6)
low = 0, high = 2, mid = 1 

is mid = target? No => Pick the correct half.

(6)
low = 0, high = 0, mid = 0

is mid = target? Yes

If the target doesn't exist, at the end we'll have high < low which means the search space is over thus the target doesn't exist.

##### Iterative Binary Search
```java
public static int binarySearch(int[] array, int target){
	int low = 0, high = array.length - 1;
	
	while (low <= high){
		int mid = (low + high) / 2;

		if (array[mid] == target) { return mid; }  // Target found
		else if (target > array[mid]){  // Target is on the right half
			low = mid + 1;
		}
		else {  // Target is on the left half
			high = mid - 1;
		}
	}
	return -1;  // Target isn't in the array
}
```
**TIME  COMPLEXITY:** O(LOG N)
**SPACE COMPLEXITY:** O(1)

##### Recursive Binary Search
```java
public static int binarySearch(int[] array, int low, int high, int target){
	if (low > high){ return -1; }  // Base case

	int mid = (low + high) / 2;
	if (target == mid){ return mid; }
	else if (target > mid){ 
		return binarySearch(array, mid + 1, high, target);
	}
	else {
		return binarySearch(array, low, mid - 1, target);
	}
}
```
**TIME  COMPLEXITY:** O(LOG N)
**SPACE COMPLEXITY:** O(1)

**NOTE:** Whenever you're cutting something by half its the inverse of exponential which means logarithmic. 

##### Overflow Case
If the size of our initial search space is Integer maximum and the target is on the last position (integer max) then on the last iteration of binary search we'll have mid = (low + high) / 2 where low = high = integer maximum and mid will overflow if its a normal integer.

Work around: mid = low + (high - low) / 2 or declare mid as long long type.

**NOTE:** Knowing **lower bound** and **higher bound** will let us solve most of the problems related to 1D arrays with binary search.

### Lower Bound
Find the first element index that satisfies the condition:
smallest index such that array(index) >= target.

```java
public static int lowerBound(int[] array, int target){
	int low = 0, high = array.length - 1;
	int answer = array.length;
	
	while (low <= high){
		int mid = (low + high) / 2;

		if (array[mid] >= target){  // Check if it can be an answer
			answer = mid;
			high = mid - 1;
		}
		else {
			low = mid + 1;
		}
	}
	return answer;
}
```
**TIME  COMPLEXITY:** O(LOG N)
**SPACE COMPLEXITY:** O(1)

### Higher Bound
Find the first element index that satisfies the condition:
smallest index such that array(index) > target.

```java
public static int upperBound(int[] array, int target){
	int low = 0, high = array.length - 1;
	int answer = array.length;
	
	while (low <= high){
		int mid = (low + high) / 2;

		if (array[mid] > target){  // Check if it can be an answer
			answer = mid;
			high = mid - 1;
		}
		else {
			low = mid + 1;
		}
	}
	return answer;
}
```
**TIME  COMPLEXITY:** O(LOG N)
**SPACE COMPLEXITY:** O(1)

---
---
## Exercises on 1D Arrays
### Search Insert Position
Find the index of the target value, if there's no element == target in the array then return the index where the element would be placed in order to keep the order of the array.

```java
public static int searchInsertPosition(int[] array, int target){
	int low = 0, high = array.length - 1;
	int answer = array.length;

	while (low <= high){
		int mid = (low + high) / 2; 

		if (array[mid] >= target){  // Check if it can be an answer
			answer = mid;
			high = mid - 1;
		}
		else {
			low = mid + 1;
		}
	}
	return answer;
}
```
**TIME  COMPLEXITY:** O(LOG N)
**SPACE COMPLEXITY:** O(1)

---
### Floor and Ceil in Sorted Array
Floor -> largest number in array <= target
Ceil -> smallest number in array >= target

array = (10 20 30 40 50), target = 25
floor = 20, ceil = 30

```java
public static Pair<Integer, Integer> findFloorAndCeil(int[] array, int target){
	int low = 0, high = array.length - 1;
	int floor = -1, ceil = -1;

	while (low <= high){
		int mid = (low + high) / 2;

		if (array[mid] >= target){
			ceil = array[mid];
			high = mid - 1;
		}
		else {
			low = mid + 1;
		}
	}

	low = 0;
	high = array.length - 1;
	while (low <= high){
		int mid = (low + high) / 2;

		if (array[mid] <= target){
			floor = array[mid];
			low = mid + 1;
		}
		else {
			high = mid - 1;
		}
	}

	return Pair<>(floor, ceil);
}
```
**TIME  COMPLEXITY:** O(2LOG N)
**SPACE COMPLEXITY:** O(1)

---
### Find the first or last occurrence of a given number in a sorted array
```java
public int[] searchRange(int[] array, int target){
	int low = 0, high = array.length - 1;
	int first = -1, last = -1;

	while (low <= high){
		int mid = (low + high) / 2;

		if (array[mid] == target){
			first = mid;
			high = mid - 1;
		}
		else if (array[mid] > target){
			high = mid - 1;
		}
		else {
			low = mid + 1;
		}
	}

	low = 0;
	high = array.length - 1;
	while (low <= high){
		int mid = (low + high) / 2;

		if (array[mid] == target){
			last = mid;
			low = mid + 1;
		}
		else if(array[mid] < target){
			low = mid + 1;
		}
		else {
			high = mid - 1;
		}
	}
	return new int[]{first, last};
    }
```
**TIME  COMPLEXITY:** O(2LOG N)
**SPACE COMPLEXITY:** O(1)

---
### Count Occurrences in Sorted Array
```java
public int[] countOccurrences(int[] array, int target){
	int low = 0, high = array.length - 1;
	int first = -1, last = -1;

	while (low <= high){
		int mid = (low + high) / 2;

		if (array[mid] == target){
			first = mid;
			high = mid - 1;
		}
		else if (array[mid] > target){
			high = mid - 1;
		}
		else {
			low = mid + 1;
		}
	}

	low = 0; high = array.length - 1;
	while (low <= high){
		int mid = (low + high) / 2;

		if (array[mid] == target){
			last = mid;
			low = mid + 1;
		}
		else if (array[mid] < target){
			low = mid + 1;
		}
		else {
			high = mid - 1;
		}
	}

	if (first != -1 && last != -1){
		return last - first + 1;  // It's important to add + 1
	}
	else { return -1; }
}
```
**TIME  COMPLEXITY:** O(2LOG N)
**SPACE COMPLEXITY:** O(1)

---
### Search element in rotated sorted array I (Unique Elements)

Identify the sorted half and then check if the target element is in that half or not, then, take the needed calculation with the low/high pointers.
```java
public static int searchRotatedSortedArrayI(int[] array, int target){
	int low = 0, high = array.length - 1;
	 
	while (low <= high){
		int mid = (low + high) / 2;
		if (array[mid] == target) { return mid; }

		if (array[low] <= array[mid]){  // Left half is sorted
			if (array[low] <= target && target <= array[mid]){  // Target is on the left half
				high = mid - 1;
			}
			else {
				low = mid + 1;
			}
		}
		else {  // Right half is sorted
			if (array[mid] <= target && target <= array[high]){  // Target is on the right half
				low = mid + 1;
			}
			else {
				high = mid - 1;
			}
		}
	}
	return -1;
}
```
**TIME  COMPLEXITY:** O(LOG N)
**SPACE COMPLEXITY:** O(1)

---
### Search element in rotated sorted array II (Non-Unique Elements)
It works the exact same way as the previous one but it has an edge-case when array: (1, 0, 1, 1, 1) in this case, the first iteration has:

low = 0  -> array(0) = 1
high = 4 -> array(4) = 1
mid = 2  -> array(2) = 1

If we try to find the sorted half like before through array(low) <= array(mid) then it will be true but the left half isn't sorted.

So we add the following condition, before checking for the correct sorted half.
```java
if (array[low] == array[mid] && array[mid] == array[high]){
	low++;
	high--;
	continue;
}
```

---
### Find minimum in rotated sorted array
With these 2 examples we can check both conditions:

Array = (8 0 1 2 3 4 5 6 7)
         l       m       h

Check which half is sorted: array(l) <= array(m).
If left side is sorted  => the smallest element is at array(l).
If right side is sorted => the smallest element is at array(m).

Then, the possible smallest element is in the unsorted half.
So we need to eliminate the sorted half, in this case: h = mid - 1.

Array = (8 0 1 2)
         l m   h

array(l) <= array(m)? -> No => Right side is sorted and we know the smallest element is at array(m) since the right side is sorted so we check if (array(m) < min) then update min. And since the right side is sorted we have to cut it (h = mid - 1)

Array = (8)
         l = h = m

array(l) <= array(m)? -> Yes => Left side is sorted => if (array(l) < min) -> Update min. Since the left side is sorted we cut it (l = mid + 1)

Array = (8) -
         h  l

Since low is greater than high, the loop breaks.

```java
public static int minimumRotatedSortedArray(int[] array){
	int low = 0, high = array.length - 1;
	int min = -1;

	while (low <= high){
		int mid = (low + high) / 2;

		if (min == -1){ min = mid; }
		
		if (array[low] <= array[mid]){  // Left half sorted
			if (array[low] < array[min]) { min = low; }
			low = mid + 1;
		}
		else {  // Right half sorted 
			if (array[mid] < array[min]) { min = mid; }
			high = mid - 1;
		}
	}
	return array[min];
}
```
**TIME  COMPLEXITY:** O(LOG N)
**SPACE COMPLEXITY:** O(1)

---
### Find how many times has the array been rotated

It's the exact same as finding the minimum element in the array but return the index.

---
### Find the single element in the sorted array

##### Brute Force Approach
Check if for each element in the array (i) the left or the right elements (i-1) or (i+1) have to be equals to (i). 
Watch out for edge cases
- The first element: if (i+1) is not similar than its the element at 0.
- The last element: if (i-1) is not similar than its the element at n-1.
- If array size is 1 then the element is array(0).

**TIME  COMPLEXITY:** O(N)
**SPACE COMPLEXITY:** O(1)

##### Binary Search Approach
The intuition is, before the single element, pairs of duplicate elements are placed at (even, odd) positions. After the single element they're placed at (odd, even) positions.
Based on this we can eliminate the halves.

Notice how for (i) if (i) is the single element then (i-1)!=(i)!=(i+1), while if its not then either (i-1) == (i) or (i+1) == (i).

So for the current position (mid) we need to verify if its at an even or odd position. And based on that verify if the right or left positions are equals to it, based on that we can eliminate the halves by moving the pointers.
```java
public static int singleNonDuplicate(int[] array){
	if (nums.length == 1){ return nums[0]; } // array has 1 element
	if (nums[0] != nums[1]){ return nums[0]; }  // first element is the single
	if (nums[nums.length - 1] != nums[nums.length -2]){ return nums[nums.length - 1]; }  // last element is the single elem

	int low = 1, high = nums.length - 2;
	while (low <= high){
		int mid = (low + high) / 2;
		if (nums[mid-1] != nums[mid] && nums[mid] != nums[mid+1]){  // found the single element
			return nums[mid];
		}

		if (mid % 2 == 0){  // if at even position
			if (nums[mid] == nums[mid+1]){  // currently at the left side of the single elem
				low = mid + 1;
			}
			else {
				high = mid - 1;
			}
		}
		else {  // if at odd position
			if (nums[mid] == nums[mid-1]){  // currently at the right side of the single elem
				low = mid + 1;
			}
			else {
				high = mid - 1;
			}
		}
	}
	return -1;
}
```
**TIME  COMPLEXITY:** O(LOG N)
**SPACE COMPLEXITY:** O(1)

---
---
## Exercises on Answers
### Find square root of a number in log n
We know that the range of answers is between 1 and n. So we can apply binary search to that.
```java
public static int squareRootBS(int n){
	int low = 1, high = n;
	int ans = 1;
	
	while (low <= high){
		int mid = (low + high) / 2;

		if (mid * mid <= n){
			answer = mid;
			low = mid + 1;  // verify if there's a greater root square number
		}
		else {
			high = mid - 1;
		}
	}
	return ans;
}
```
**TIME  COMPLEXITY:** O(LOG N)
**SPACE COMPLEXITY:** O(1)

---
### Find the Nth root of a number

```java
public static int nthRootBS(int n, int number){
	if (number == 0){ return 0; }
	if (number == 1){ return 1; }
	
	int low = 1, high = number;
	int ans = -1;

	while (low <= high){
		int mid = (low + high) / 2;
		int aux = 1;
		
		for (int i = 0; i < n; i++){
			aux *= mid;
		}

		if (aux == number) { return mid; }
		if (aux < number){
			ans = mid;
			low = mid + 1;
		}
		else {
			high = mid - 1;
		}
	}
	return ans;
}
```
**TIME  COMPLEXITY:** O(N LOG M) - N being the loop for calculation and M the max range of BS
**SPACE COMPLEXITY:** O(1)

---
### Koko eating bananas

```java
public static int kokoBananasBS(int[] piles, int h){
	int low = 1, high = -1, ans = - 1;
	for (int pile : piles){
		if (pile > high){
			high = pile;
		}
	}

	while (low <= high){
		int mid = low + (high - low) / 2;
		long time = 0;  // overflow test-case

		// time calculation calculation can be optimised with
		// time += (pile + (long)mid - 1) / mid;
		for (int pile : piles){  
			time = time + (pile / mid);

			if (pile % mid != 0){
				time += 1;
			}
		}
		
		// since we're looking for the minimum banana/hour rate and for
		// current mid its within the amount of hours defined to eat the
		// bananas then it means the numbers on the left half (up until mid)
		// might be capable of accomplishing it too and since we're looking for 
		// the minimum banana/hour rate we eliminate the right half since all
		// of those will accomplish the goal
		if (time <= h){
			ans = mid;
			high = mid - 1;  
			
		}
		else {
			low = mid + 1;
		}
	}
	return ans;
}
```
**TIME  COMPLEXITY:** O(N LOG M) - N being the loop over the piles array to find the max and M the max range of BS
**SPACE COMPLEXITY:** O(1)

---
### Find the smallest divisor given a threshold

```java
public static int smallestDivisor(int[] nums, int threshold) {
	int low = 1, high = -1, ans = - 1;
	for (int num : nums){
		if (num > high){
			high = num;
		}
	}
	
	while (low <= high){
		int mid = low + (high - low) / 2;
		long count = 0;
		for (int num : nums){
			count = count + (num + (long) mid - 1) / mid;
		}

		if (count <= threshold){  
			ans = mid;
			high = mid - 1;  // we want to find a smaller possible divisor
		}
		else {
			low = mid + 1;
		}
	}
	return ans;  // or return low
}
```
**TIME  COMPLEXITY:** O(N LOG M) - N being the loop over the piles array to find the max and M the max range of BS
**SPACE COMPLEXITY:** O(1)

---
### Find Kth missing positive number
Say an array like `arr[] = [2, 3, 4, 7, 11]` and `k = 5`.
The maximum of the array is 11 -> `[1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11]` and the missing numbers are: `[1, 5, 6, 8, 9, 10]` therefore the 5th missing number is 9.

Observation:
If the array starts from a number > k then the k-th missing number is k itself.
`arr[] = [5, 7, 10, 12, 13]` -> `k = 4` => k-th missing number is 4.

We can not directly apply binary search to this because the answer range is fragmented. But 
```java
public static int findKthPositive(int[] arr, int k) {
	int missingCounter = arr[i] - 1;
	if (missingCounter >= k){ return k; }

	int low = -1, high = -1, ans = -1;
	for (int i = 1; i < arr.length; i++){
		if (arr[i] != arr[i-1] + 1){
			missingCounter += arr[i] - arr[i-1] - 1;
		}

		if (missingCounter >= k){
			low = arr[i-1];
			high = arr[i];
			missingCounter -= arr[i] - arr[i-1] - 1;
		}
	}

	if (low == -1 || high == -1){ return ans; }
	int aux = high - low - 1;
	while (count + aux != k){
		aux--;
	}
	return low + aux;
}
```