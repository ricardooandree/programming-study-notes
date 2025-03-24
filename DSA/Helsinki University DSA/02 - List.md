<font color="#7f7f7f"><em>Friday, 27-09-2024 - 16:38</em></font>
#programming #dsa 

### List in memory
The memory of a computer consists of a sequence of memory locations (contiguous) capable of storing data. Each memory location has an address that can be used for access.

###### Example: Declaration and initialization of variables
```python
a = 7
b = -3
c = [1, 2, 3, 1]
d = 99
```

| Addresses | 100 | 101 | 102 | 103 | 104 | 105 | 106 | 107 | 108 | 110 |
| --------- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Value     | 7   | -3  | 1   | 2   | 3   | 1   | 2   | 0   | 0   | 99  |
| Variable  | a   | b   | c   |     |     |     |     |     |     | d   |
This facilitates lots of data operations, through the memory address of an element we can access the stored element.

For example, ***c[2]*** is at the address ***102 + 2 = 104***. 

The list occupies more memory than is needed for its current elements to prepare for the possibility of addition of new elements to the list. Thus the list has two sizes: the number of elements (5) and the number of memory locations reserved for that list (8).

### List operations
Python has several built-in operations for managing lists.

##### Indexing
The elements of a list can be accessed and modified using the index operator (`[]`).
```python
numbers = [4, 3, 7, 3, 2]
numbers[2] = 5
print(numbers[2]) # 7
```
##### Size
The size of a list can be obtained using the function (`len`).
```python
numbers = [4, 3, 7, 3, 2]
print(len(numbers)) # 5
```
##### Searching
- Is element on list? => Operator (`in`)
- Search element index? => Method (`index`)
- Count occurrences of element? => Method (`count`)

How are these methods implemented?
```python
def count(items, target):
	result = 0
	for item in items:
		if item == target:
			result += 1
	return result
```
##### Adding an element
- Add to end? => Method (`append`)
- Add to a position? => Method (`insert`)

##### Removing an element
- Remove from end? => Method(`pop`)
- Remove from position? => Method(`pop`)
- Remove first entry? => Method(`remove`)

| Operation                         | Time Complexity |
|:--------------------------------- |:---------------:|
| Indexing (`[]`)                   |      O(1)       |
| Size (`len`)                      |      O(1)       |
| Is element on list? (`in`)        |      O(n)       |
| Searching (`index`)               |      O(n)       |
| Counting (`count`)                |      O(n)       |
| Adding to end (`append`)          |      O(1)       |
| Adding to position (`insert`)     |      O(n)       |
| Removing from end (`pop`)         |      O(1)       |
| Removing from position (`pop`)    |      O(n)       |
| Searching and removing (`remove`) |      O(n)       |

### References and copying
In Python, lists and other data structures are accessed through references.
```python
a = [1, 2, 3, 4]
b = a
a.append(5)
```
In the case above, the instruction b = a causes the variables a and b to refer to the same list in memory, so when appending to the variable a, it also "appends" to the variable b.

To avoid this problem we can use the method copy.
```python
b = a.copy()
```

- Reference copying => Time complexity O(1)
- Object copying => Time complexity O(n)

##### Side effects of functions
Important: When a function is given a data structure as a parameter, only a reference is copied. This can cause undesired side effects.
```python
def double(numbers):
	result = numbers
	for i in range(len(result)):
		result[i] *= 2
	return result
```
The function above modifies the contents of the data structure <u>numbers</u> on the overall state of the program.

##### Slicing and concatenation
The Python slice operator (`[:]`) creates a new list that contains a copy of a segment of the given list.

The operator (`+`) can be used to concatenate lists and copies the contents of the original lists to a new one.

### Lists in other languages
The Python list is generally known as an *array list* or a *dynamic array*.

In low level languages, the basic data structure is the *array*, this is a sequence of consecutive elements that can be accessed with indexing. However, an array is assigned a fixed memory area when it is created and its size cannot be changed later.

List in Java:
```java
ArrayList<Integer> numbers = new ArrayList<>();

numbers.add(1);
```

List in C++:
```cpp
std::vector<int> numbers;

numbers.push_back(1);
```w