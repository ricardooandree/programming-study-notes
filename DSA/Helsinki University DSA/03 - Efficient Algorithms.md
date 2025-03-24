<font color="#7f7f7f"><em>Friday, 27-09-2024 - 16:38</em></font>
#programming #dsa 

### Outline of an efficient algorithm
A typical efficient algorithm has a time complexity of O(n) or O(nlog n), the latter usually being related to sorting.

A loop may contain:
- Updates of variable values using other variables or individual elements of the input
- Arithmetic expressions related to variable updates
- If-commands that affect the variable updates

A loop may not contain:
- Other loops that go through the input
- Slow operations that process the input (e.g. count or slice operation)
- Slow function calls (e.g. sum, min or max applied to the whole input)

### Example: Stock trading
**Task:** You are given the price of a stock for ***n*** days. Your task is to figure out the highest profit you could have made if you had bought the stock on one day and sold it on another day.

Consider the following situation:

| Day   | 0   | 1   | 2   | 3   | 4   | 5   | 6   | 7   |
| ----- | --- | --- | --- | --- | --- | --- | --- | --- |
| Price | 3   | 7   | 5   | 1   | 4   | 6   | 2   | 3   |
Here the highest profit is 6 - 1 = 5, achieved by buying on day 3 and selling on day 5.

###### Implementation 1 - O(n^2)
```python
def best_profit(prices):
	n = len(prices)
	best = 0
	
	for i in range(n):
		for j in range(i+1, n):
			best = max(best, prices[j] - prices[i])
	return best
```
The algorithm loops through the buying days (i) and then loops through the selling days (j), then if the difference of prices between the selling day and the buying day is higher than the last one, update best.

###### Implementation 2 - O(n^2)
```python
def best_profit(prices):
	n = len(prices)
	best = 0
	for i in range(n):
		min_price = min(prices[0:i+1])
		best = max(best, prices[i] - min_price)
	return best
```
This algorithm loops through the selling days (i) and computes the lowest price up to day i into the variable min_price, however this creates a new list through the slicing operation which is of time complexity O(n), making the overall algorithm O(n^2).

###### Implementation 3 - O(n)
```python
def best_profit(prices):
	n = len(prices)
	best = 0
	min_price = prices[0]
	
	for i in range(n):
		min_price = min(min_price, prices[i])
		best = max(best, prices[i] - min_price)
	return best
```
In this algorithm the minimum price is calculated through the previous one and then we compare if the current best profit is higher than the one we'd have by selling on the current iteration (selling day).

### Is this algorithm correct?
Determining the correctness of a complex algorithm may be hard, one approach is to implement the algorithm through brute force and compare the results with the most efficient algorithm implementation.

### Example: Bit string
**Task:** You are given a bit string consisting of the characters 0 and 1. How many ways can you select two positions in the bit string so that the left position contains the bit 0 and the right position contains the bit 1?

| Position | 0   | 1   | 2   | 3   | 4   | 5   | 6   | 7   |
| -------- | --- | --- | --- | --- | --- | --- | --- | --- |
| Bit      | 0   | 1   | 0   | 0   | 1   | 0   | 1   | 1   |
Here there are 12 such pairs of positions.

###### Implementation 1 - Brute force - O(n^2)
```python
def count_ways(bits):
	n = len(bits)
	result = 0
	
	for i in range(n):
		for j in range(i + 1, n):
			if bits[i] == '0' and bits[j] == '1':
				result += 1
	return result
```

###### Implementation 2 - Efficient approach - O(n)
```python
def count_ways(bits):
	n = len(bits)
	result = 0
	zeros = 0
	
	for i in range(n):
		if bits[i] == '0':
			zeros += 1
		if bits[i] == '1':
			result += zeros
	return result
```
If the bit at position i is 0, the count is obviously 0.
If the bit at position i is 1, we need to know how many of the preceding positions contain a 0 bit => zeros counter.
### Example: List splitting
**Task:** You are given a list containing n integers. Your task is to count how many ways one can split the list into two parts so that both parts have the same total sum of elements.

| Position | 0   | 1   | 2   | 3   | 4   | 5   | 6   | 7   |
| -------- | --- | --- | --- | --- | --- | --- | --- | --- |
| Number   | 1   | -1  | 1   | -1  | 1   | -1  | 1   | -1  |
Here the number of ways is 3. We can split the list between positions 1 and 2, between positions 3 and 4, and between positions 5 and 6.

###### Implementation 1 - Brute force - O(n^2)
```python
def count_splits(numbers):
	n = len(numbers)
	result = 0
	
	for i in range(n - 1):
		left_sum = sum(numbers[0:i + 1])    # O(n)
		right_sum = sum(numbers[i + 1:])    # O(n)

		if left_sum == right_sum:
			result += 1
	return result
```

###### Implementation 2 - Efficient approach - O(n)
```python
def count_ways(bits):
	n = len(bits)
	result = 0
	left_sum = 0
	total_sum = sum(numbers)    # O(n)
	
	for i in range(n - 1):
		left_sum += numbers[i]
		right_sum = total_sum - left_sum
		
		if left_sum == right_sum:
			result += 1
	return result
```
We calculate the left sum based on the traversal of the list.
Then we calculate the right based on the difference between total sum and left sum.
### Example: Sub-lists
**Task:** You are given a list containing n integers. How many ways can we choose a sub-list that contains exactly two distinct integers?

For example, the list [1, 2, 3, 3, 2, 2, 4, 2] has 14 such sub-lists.

| Index  | 0   | 1   | 2   | 3   | 4   | 5   | 6   | 7   |
| ------ | --- | --- | --- | --- | --- | --- | --- | --- |
| Number | 1   | 2   | 3   | 3   | 2   | 2   | 4   | 2   |
| Count  | 0   | 1   | 1   | 1   | 3   | 3   | 2   | 3   |

**Core idea:** <u>Count the sub-lists ending at a position i</u>.

For that we'll use two variables:
- a: points to the nearest preceding position that contains a  different value than the one in the position i.
- b: points to the nearest preceding position whose value differs from both of the values at the position i and a.

These variables are important because a valid sub-list ending at i must start after the position b and before or at the position a. Thus the number of valid sub-lists ending at i can be counted with the formula a - b.

| Index  | 0   | 1   | 2   | 3   | 4   | 5   | 6   | 7   |
| ------ | --- | --- | --- | --- | --- | --- | --- | --- |
| Number | 1   | 2   | 3   | 3   | 2   | 2   | 4   | 2   |
|        | b   |     |     | a   |     | i   |     |     |
a => position: [3] => value: 3
b => position: [0] => value: 1
Count of sub-lists: 3 - 0 = 3

The variables a and b must be updated when the value at i is different than the value at i - 1.
- If the value at i is different from the value at a, b moves to the position a and a moves to the position i - 1.
- If the value at i is equal to the value at a, a again moves to i - 1 but b does not move.

Next step of our example:

| Index  | 0   | 1   | 2   | 3   | 4   | 5   | 6   | 7   |
| ------ | --- | --- | --- | --- | --- | --- | --- | --- |
| Number | 1   | 2   | 3   | 3   | 2   | 2   | 4   | 2   |
|        |     |     |     | b   |     | a   | i   |     |

| Index  | 0   | 1   | 2   | 3   | 4   | 5   | 6   | 7   |
| ------ | --- | --- | --- | --- | --- | --- | --- | --- |
| Number | 1   | 2   | 3   | 3   | 2   | 2   | 4   | 2   |
|        |     |     |     | b   |     |     | a   | i   |

###### Implementation - O(n)
```python
def count_lists(numbers):
	n = len(numbers)
	a = b = -1
	result = 0

	for i in range(1, n):
		if numbers[i] != numbers[i - 1]:
			if numbers[i] != numbers[a]:
				b = a
			a = i - 1
		result += a - b
	return result
```
Initially, both a and b have the value -1, which indicates that they are not yet pointing to any list position. It is straightforward to verify that the algorithm computes the correct sub-list counts in the beginning while a or b still has the value -1.