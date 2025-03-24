<font color="#7f7f7f"><em>Friday, 27-09-2024 - 16:38</em></font>
#programming #dsa 

### What is an algorithm? 
An algorithm is a method/procedure for solving a problem. In the context of programming, it is a set of instructions that perform a specific computational task based on a certain input.

![[Algorithm | 1000 | center]]

###### Example: Algorithm to count even numbers in a list
```python
def count_even(numbers):
	result = 0
	for x in numbers:
		if x % 2 == 0:
			result += 1
	return result
```

### What is a data structure?
A data structure defines how data is organized, stored and manipulated/processed within a program.

![[Data Structures | 1000 | center]]

### Implementing an algorithm
In order to implement a computational algorithm we use a set of basic programming constructs:
- Variables | Operators | Conditionals | Loops | Lists | Functions | Classes

### Efficiency of algorithms
The same task can be solved by different algorithms which leads to differences in their efficiencies, it's very important to implement efficient algorithms.

###### Example: Algorithm 1
```python
def max_diff(numbers):
	result = 0
	for x in numbers:
		for y in numbers:
			result = max(result, abs(x - y))
	return result
```

###### Example: Algorithm 2
```python
def max_diff(numbers):
	numbers = sorted(numbers)
	return numbers[-1] - numbers[0]
```

###### Example: Algorithm 3
```python
def max_diff(numbers):
	return max(numbers) - min(numbers)
```

##### Measuring efficiency
To measure the efficiency of an algorithm we can write a test program that runs the algorithm for a given input and measures the execution time for different sized inputs.
```python
n = 1000
print("n: ", n)
random.seed(1337)
numbers = [random.randint(1, 10**6) for _ in range(n)]

start_time = time.time()
result = max_diff(numbers)
end_time = time.time()

print ("result: ", result)
print("time: ", round(end_time - start_time, 2), "s")
```

Execution:
`n:1000`
`result: 999266`
`time: 0.09s`

Table with different execution times of the three algorithms:

| List length `n` | Algorithm 1 | Algorithm 2 | Algorithm 3 |
| --------------- | ----------- | ----------- | ----------- |
| 1000            | 0.17 s      | 0.00 s      | 0.00 s      |
| 10000           | 15.93 s     | 0.00 s      | 0.00 s      |
| 100000          | –           | 0.01 s      | 0.00 s      |
| 1000000         | –           | 0.27 s      | 0.02 s      |

### Analysis of algorithms
The efficiency of an algorithm can be estimated by counting how many steps the algorithm executes for an input of a given size instead of exploring runtime execution.

```python
def count_even(numbers):
	result = 0
	for x in numbers:
		if x % 2 == 0:
			result += 1
	return result
```

Let ***n*** denote the length of the list. Since the algorithm goes through all elements of the list, the number of steps depends on ***n***.
- Lines 2 and 6 are executed once.
- Lines 3 and 4 are executed ***n*** times.
- Line 5 is executed at least ***0*** times and most ***n*** times.

Thus the algorithm executes at least ***2n + 2*** steps and at most ***2n + 3*** steps.

##### Time complexity
Instead of exploring run-time execution or the exact number of steps we can explore time complexity, which gives the magnitude of the number of steps on a given input size.

Usually shown in the form of O(*f(n)*) where *f(n)* is replaced by an arithmetic expression representing an <u>upper bound</u> of the number of steps.
For instance, for the algorithm that executes ***2n + 3*** steps at most, *f(n) = n => O(n)*. 

| Time complexity | Name of complexity class |
| :-------------: | :----------------------: |
|      O(1)       |         Constant         |
|    O(log⁡ n)    |       Logarithmic        |
|      O(n)       |          Linear          |
|   O(nlog ⁡n)    |            –             |
|     O(n^2)      |        Quadratic         |
|     O(n^3)      |          Cubic           |
In practice time complexity is often determined by the loops in the code:
- No loops => Constant time => *O(1)*
- Single loop => *O(n)*
- Nested loops => *O(n^2)*