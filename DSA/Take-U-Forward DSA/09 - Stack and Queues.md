<font color="#7f7f7f"><em>Friday, 01-11-2024 - 15:28</em></font>

## Index
- [[#Theoretical Fundaments]]
- [[#Monotonic Stack]]
- [[#Exercises]]
- [[#Prefix, Infix and Postfix Conversions]]

## Theoretical Fundaments
##### What are Stacks and Queues?
**Stack**: Last In First Out (*LIFO*) -> `Stack<Integer> stack`
**Queue**: First In Fist Out (*FIFO*) -> `Queue<Integer> queue`

**Methods/Functions**
- push(x) - Inserts x in the stack/queue
- pop()   - Removes an element from the stack/queue
- top()   - Removes the top element from the stack/queue
- size()  - Gives the size of the stack/queue

![[Stack and Queue Data Structure| center | 1000]]

### Stack/Queue Implementation - Arrays
Since we are using arrays it means the Stack/Queue will be static and we need a constant size.
We use a **top** pointer to track the elements/size in the array, starting off with top at -1 and once a push happens, move it to index 0 and store the value and so on.

```java
class Stack {
	private int top;
	private int[] st;

	public Stack(int size){
		this.top = -1;
		this.st = new int[size];
	}

	public void push(int x){
		if (top == st.length - 1){  // Stack is full
			throw new RuntimeException("Stack is full");
		}

		top++;
		st[top] = x;
	}

	public void pop(){
		if (top == -1){  // Stack is empty
			throw new RuntimeException("Stack is empty");
		}
		top--;
	}
	
	public int top(){
		if (top == -1){  // Stack is empty
			throw new RuntimeException("Stack is empty");
		}

		return st[top];
	}

	public int size(){
		return top + 1;
	}
}
```

Queues are more delicate to handle than Stacks and need two pointers for tracking as well as a variable to track the current size of since since the end pointer is not representative of the Queue size.
```java
class Queue {
	private int currentSize;
	private int start, end;
	private int[] q;

	public Queue(int size){
		this.start = -1;
		this.end = -1;
		this.currentSize = 0;
		this.q = new int[size];
	}

	public void push(int x){
		if (currentSize == q.length){  // Queue is full
			throw new RuntimeException("Queue is full");
		}
		
		if (currentSize == 0){  // Queue is empty - First position
			start = 0;
			end = 0;
		}
		else {                  // Queue is not empty
			end = (end + 1) % q.length;
		}
		
		q[end] = x;
		currentSize++;
	}

	public int pop(){
		if (currentSize == 0){  // Queue is empty
			throw new RuntimeException("Queue is empty");
		}

		int elem = q[start];
		if (currentSize == 1){  // Queue has 1 element
			start = -1;
			end = -1;
		}
		else {
			start = (start + 1) % q.length;  // Moves start pointer forward
		}

		currentSize--;
		return elem;
	}
	
	public int top(){
		if (currentSize == 0){  // Queue is empty
			throw new RuntimeException("Queue is empty");
		}

		return q[start];
	}

	public int size(){
		return currentSize;
	}
}
```
<u>TIME COMPLEXITY</u>:  O(1)
<u>SPACE COMPLEXITY</u>: O(N)

### Stack/Queue Implementation - LinkedList
```java
class Stack {
	private int size;
	private Node top;
	
	public Stack(){
		this.size = 0;
		this.top = null;
	}

	public void push(int x){         // Create a new node and make it the head
		Node temp = new Node(x);     // of the list aka top
		temp.next = top;            
		top = temp; 
		size++;
	}

	public void pop(){               // Remove head node
		if (top == null){
			throw new RuntimeException("Stack is empty");
		}
		
		top = top.next;
		size--;
	}
	
	public int top(){
		if (top == null){
			throw new RuntimeException("Stack is empty");
		}
		
		return top.data;
	}

	public int size(){
		return size;
	}
}
```

```java
class Queue {
	private int size;
	private Node start, end;

	public Queue(){
		this.size = 0;
		this.start = null;
		this.end = null;
	}
		
	public void push(int x){
		Node temp = new Node(x);
	
		if (start == null){  // Push first node into the queue
			start = temp;
			end = temp;
		}
		else {
			end.next = temp;
			end = temp;
		}

		size++;
	}

	public void pop(){
		if (start == null){
			throw new RuntimeException("Queue is empty");
		}

		start = start.next;
		size--;

		if (start == null){  // If queue is now empty, reset `end`
			end = null; 
		}
	}
	
	public int top(){
		if (start == null){
			throw new RuntimeException("Stack is empty");
		}
		
		return start.data;
	}

	public int size(){
		return size;
	}
}
```
<u>TIME COMPLEXITY</u>:  O(1)
<u>SPACE COMPLEXITY</u>: O(1)

### Stack Implementation - Queue
This is a tricky implementation, we need to reverse the order of the queue so it becomes a stack. For that, whenever we add a new element to the Queue we shift all the elements on the list so that the last inserted element always stays at the "end".

```java
class StackUsingQueue {
	private Queue<Integer> q;

	public StackUsingQueue(){
		q = new LinkedList<>();
	}

	public void push(int x){     
		q.push(x);
		
		int size = q.size();
		while (size > 1){
			q.push(q.top());
			q.pop();
			size--;
		}
	}

	public void pop(){
		q.pop();  
	}
	
	public int top(){
		return q.top();
	}

	public int size(){
		return q.size();
	}
}
```
<u>TIME COMPLEXITY</u>:  O(N)
<u>SPACE COMPLEXITY</u>: O(N)

### Queue Implementation - Stack
We need two stacks (Dequeue) that act as follows:

**Approach 1:** Make push operation time expensive by doing the transfer.
**Push**:
- Put everything from S1 into S2.
- Put x in S1.
- Move everything from S2 into S1.

**Approach 2:** Make pop/top operation time expensive by doing the transfer.
**Pop/Top:** 
- If S2 is not empty -> Pop/Top from it.
- Else -> Move everything from S1 into S2 and Pop/Top from S2.
```java
class QueueUsingStack {
	private Stack<Integer> s1, s2;

	public QueueUsingStack {
		s1 = null;
		s2 = null;
	}

	public void push(int x){         
		// Approach 1           
		// Put everything from S1 into S2
		for (int i = 0; i < s1.size(); i++){
			s2.push(s1.top());
			s1.pop();
		}

		// Put x into S1
		s1.push(x);

		// Put everything from S2 into S1
		for (int i = 0; i < s2.size(); i++){
			s1.push(s2.top());
			s2.pop();
		}


		s1.push(x);  // Approach 2
	}

	public void pop(){
		s1.pop();  // Approach 1

		if (s2.size() != 0){  // Approach 2
			s2.pop();
		}
		else {
			for (int i = 0; i < s1.size(); i++){
				s2.push(s1.top());
				s1.pop();
			}

			s2.pop();
		}
	}

	public int top(){
		s1.top();  // Approach 1

		// Approach 2 - same as pop
	}

	public int size(){
		s1.size();
	}
}
```
<u>TIME COMPLEXITY</u>:  O(N)
<u>SPACE COMPLEXITY</u>: O(2N)

### Monotonic Stack
When you store the elements on a stack in a specific order such as increasing or decreasing.

It's very used in exercises such has:
- NEXT => moving backwards in the array.
- PREVIOUS => moving forward in the array.
- SMALLER => looking for the next/previous smaller number **(Pop if current <= top)**.
- GREATER => looking for the next/previous greater number **(Pop if current >= top) - pop the stack because a new greater appeared**.

##### Next Greater Element (NGE) -> Backwards + current >= top
Stack Order: Monotonic Decreasing (Top = Largest)  
Traverse Right to Left (Backwards)

**Input:**  [3, 1, 2, 4]
**Output:** [4, 2, 4, -1]

```java
for (int i = nums.length - 1; i >= 0; i--){
	while (!st.isEmpty() && nums[i] >= st.peek()){
		st.pop();
	}

	answer[i] = st.isEmpty() ? -1 : st.peek();
	st.push(nums[i]);
}
```

##### Previous Greater Element (PGE) -> Forward + current >= top
Stack Order: Monotonic Decreasing (Top = Largest)  
Traverse Left to Right (Forward)

**Input:**  [3, 1, 2, 4]
**Output:** [-1, 3, 3, -1]
```java
for (int i = 0; i <= nums.length; i++){
	while (!st.isEmpty() && nums[i] >= st.peek()){
		st.pop();
	}

	answer[i] = st.isEmpty() ? -1 : st.peek();
	st.push(nums[i]);
}
```

##### Next Smaller Element (NSE) -> Backwards + current <= top
Stack Order: Monotonic Increasing (Top = Smallest)  
Traverse Right to Left (Backwards)

**Input:**  [3, 1, 2, 4]
**Output:** [1, -1, -1, -1]
```java
for (int i = nums.length - 1; i >= 0; i--){
	while (!st.isEmpty() && nums[i] <= st.peek()){
		st.pop();
	}

	answer[i] = st.isEmpty() ? -1 : st.peek();
	st.push(nums[i]);
}
```

##### Previous Smaller Element (PSE) -> Forward + current <= top
Stack Order: Monotonic Increasing (Top = Smallest)  
Traverse Left to Right (Forward)

**Input:**  [3, 1, 2, 4]
**Output:** [-1, -1, 1, 2]
```java
for (int i = 0; i <= nums.length; i++){
	while (!st.isEmpty() && nums[i] <= st.peek()){
		st.pop();
	}

	answer[i] = st.isEmpty() ? -1 : st.peek();
	st.push(nums[i]);
}
```

---
---
## Exercises
### Valid Parentheses 
Given a string `s` containing just the characters `'('`, `')'`, `'{'`, `'}'`, `'['` and `']'`, determine if the input string is valid.

An input string is valid if:

1. Open brackets must be closed by the same type of brackets.
2. Open brackets must be closed in the correct order.
3. Every close bracket has a corresponding open bracket of the same type.
```java
public boolean isValid(String s){
	Stack<Character> stack = new Stack<>();

	for (char c : s.toCharArray()){
		if (c == '(' || c == '[' || c == '{'){
			stack.push(c);
		}
		else {
			// String starts with closing bracket
			if (stack.isEmpty()){ return false; }  

			char open = stack.pop();
			if (open == '(' && c != ')' ||
				open == '[' && c != ']' ||
				open == '{' && c != '}'){
				
				return false;
			}	
		}
	}

	return stack.isEmpty();  // String is '((' or has more opening brackets
}
```
<u>TIME COMPLEXITY</u>:  O(N) - Iterates through the string
<u>SPACE COMPLEXITY</u>: O(N) - Uses a stack

---
### Min Stack
Design a stack that supports push, pop, top, and retrieving the minimum element in constant time.

Implement the `MinStack` class:

- `MinStack()` initializes the stack object.
- `void push(int val)` pushes the element `val` onto the stack.
- `void pop()` removes the element on the top of the stack.
- `int top()` gets the top element of the stack.
- `int getMin()` retrieves the minimum element in the stack.

You must implement a solution with `O(1)` time complexity for each function.

```java
class MinStack {
	private Stack<Integer> stack, minStack;

	public MinStack(){
		stack = new Stack<>();
		minStack = new Stack<>();
	}

	public void push(int val){
		stack.push(val);

		if (minStack.isEmpty() || val <= minStack.peek()){
			minStack.push(val);
		}
	}

	public void pop(){
		if (stack.peek().equals(minStack.peek())){
			minStack.pop();
		}

		stack.pop();
	}

	public int top(){
		return stack.peek();
	}

	public int getMin(){
		return minStack.peek();
	}
}
```
<u>TIME COMPLEXITY</u>:  O(1)
<u>SPACE COMPLEXITY</u>: O(2N)

---
### Next Greater Element I - Monotonic Stack
**Input:** nums1 = [4,1,2], nums2 = [1,3,4,2]
**Output:** [-1,3,-1]
**Explanation:** The next greater element for each value of nums1 is as follows:
- 4 is underlined in nums2 = [1,3,4,2]. There is no next greater element, so the answer is -1.
- 1 is underlined in nums2 = [1,3,4,2]. The next greater element is 3.
- 2 is underlined in nums2 = [1,3,4,2]. There is no next greater element, so the answer is -1.

```java 
public int[] nextGreaterElement(int[] nums1, int[] nums2){
	Stack<Integer> st = new Stack<>();
	int[] ngeNums2 = new int[nums2.length];

	for (int i = nums2.length - 1; i >= 0; i--){
		while (!st.isEmpty() && nums2[i] >= st.peek()){
			st.pop();
		}

		ngeNums2[i] = st.isEmpty() ? -1 : st.peek();
		st.push(nums2[i]);
	}

	HashMap<Integer, Integer> ngeNums2Map = new HashMap<>();
	for (int i = 0; i < nums2.length; i++){
		ngeNums2Map.put(nums2[i], ngeNums2[i]);
	}

	int[] answer = new int[nums1.length];
	for (int = 0; i < nums1.length; i++){
		answer[i] = map.get(nums1[i]);
	}

	return answer;
}
```
<u>TIME COMPLEXITY</u>:  O(N + 2M)
<u>SPACE COMPLEXITY</u>: O(N + 2M)

---
### Daily Temperatures
Same intuition has NGE but needs to store the indexes instead of the actual values to count the difference to the NGE.

**Input:** temperatures = [73,74,75,71,69,72,76,73]
**Output:** [1,1,4,2,1,1,0,0]

```java
public int[] dailyTemperatures(int[] temperatures){
	Stack<Integer> st = new Stack<>();
	int[] answer = new int[temperatures.length];

	for (int i = temperatures.length - 1; i >= 0; i--){
		while (!st.isEmpty() && temperatures[i] >= temperatures[st.peek()]){
			st.pop();
		}

		answer[i] = st.isEmpty() ? 0 : st.peek() - 1;

		st.push(i);
	}

	return answer;
}
```
<u>TIME COMPLEXITY</u>:  O(N)
<u>SPACE COMPLEXITY</u>: O(N)

---
### Next Greater Element II
Given a circular integer array `nums` (i.e., the next element of `nums[nums.length - 1]` is `nums[0]`), return _the **next greater number** for every element in_ `nums`.

The **next greater number** of a number `x` is the first greater number to its traversing-order next in the array, which means you could search circularly to find its next greater number. If it doesn't exist, return `-1` for this number.

**Input:** nums = [1,2,1]
**Output:** [2,-1,2]
Explanation: The first 1's next greater number is 2; 
The number 2 can't find next greater number. 
The second 1's next greater number needs to search circularly, which is also 2.

PROBLEM: Circular array.
INTUITION: Can either run the algorithm twice to simulate a copy of the original array and one cycle through it to cover all cases or use the % operator for undefined cycles.

```java
public int[] nextGreaterElements(int[] nums){
	Stack<Integer> st = new Stack<>();
	int[] answer = new int[nums.length];

	for (int i = 2 * nums.length - 1; i >= 0; i--){
		while (!st.isEmpty() && nums[i % nums.length] >= st.peek()){
			st.pop();
		}

		if (i < nums.length){
			answer[i] = st.isEmpty ? -1 : st.peek();
		}

		st.push(nums[i % nums.length]);
	}

	return answer; 
}
```
<u>TIME COMPLEXITY</u>:  O(2N)
<u>SPACE COMPLEXITY</u>: O(N)

---
### Asteroid Collision
We are given an array `asteroids` of integers representing asteroids in a row. The indices of the asteroid in the array represent their relative position in space.

For each asteroid, the absolute value represents its size, and the sign represents its direction (positive meaning right, negative meaning left). Each asteroid moves at the same speed.

Find out the state of the asteroids after all collisions. If two asteroids meet, the smaller one will explode. If both are the same size, both will explode. Two asteroids moving in the same direction will never meet.

**Input:** asteroids = [5,10,-5]
**Output:** [5,10]
**Explanation:** The 10 and -5 collide resulting in 10. The 5 and 10 never collide.

**Input:** asteroids = [8,-8]
**Output:** []
**Explanation:** The 8 and -8 collide exploding each other.

**Input:** asteroids = [10,2,-5]
**Output:** [10]
**Explanation:** The 2 and -5 collide resulting in -5. The 10 and -5 collide resulting in 10.

INTUITION: Push asteroids into a stack and once the current asteroid is negative then it means a collision unless the stack is empty. After that check which asteroid wins the collision based on its size => popping or not.

```java
public int[] asteroidCollision(int[] asteroids) {
	Stack<Integer> st = new Stack<>();

	for (int a : asteroids){
		// if asteroid is positive => never collides
		if (a > 0){
			st.push(a);
		}
		else {
			// asteroid is negative (possible collision)
			// if stack is empty there is no need to pop
			// if stack top is < 0 there is no need to pop
			// if the absolute value of a is smaller than stack top don't pop
			// => if stack isnt empty and the top is negative and smaller
			// value than the asteroid then pop
			while (!st.isEmpty() && st.peek() > 0 && st.peek() < -a){
				st.pop();
			}

			// if the stack is empty or the top is negative 
			// then the current negative asteroid should be added (no collision)
			if (st.isEmpty() || st.peek() < 0){
				st.push(a);
			}

			// if stack top == a value then pop (both destroyed)
			if (st.peek() == -a){
				st.pop();
			}
		}
	}

	int[] result = new int[st.size()];
	for (int i = result.length - 1; i >= 0; i--){
		result[i] = st.pop();
	}

	return result;
}
```
<u>TIME COMPLEXITY</u>:  O(2N)
<u>SPACE COMPLEXITY</u>: O(2N)

---
### Evaluate Reverse Polish Notation (Postfix)
You are given an array of strings `tokens` that represents an arithmetic expression in a [Reverse Polish Notation](http://en.wikipedia.org/wiki/Reverse_Polish_notation).

Evaluate the expression. Return _an integer that represents the value of the expression_.

**Note** that:

- The valid operators are `'+'`, `'-'`, `'*'`, and `'/'`.
- Each operand may be an integer or another expression.
- The division between two integers always **truncates toward zero**.
- There will not be any division by zero.
- The input represents a valid arithmetic expression in a reverse polish notation.
- The answer and all the intermediate calculations can be represented in a **32-bit** integer.

**Input:** tokens = ["2","1","+","3","*"]
**Output:** 9
**Explanation:** ((2 + 1) * 3) = 9

NOTE: Try-catch is a very expensive operation.

```java
public int evalRPN(String[] tokens){
	Stack<Integer> st = new Stack<>();

	for (String token : tokens){
		switch (token){
			case "+":
				st.push(st.pop() + st.pop()); break;
				
			case "-":
				int operand2 = st.pop();
				int operand1 = st.pop();
				st.push(operand1 - operand2); break;
				
			case "*":
				st.push(st.pop() * st.pop()); break;
				
			case "/":
				int divisor = st.pop();
				int dividend = st.pop();
				st.push(dividend / divisor); break;
				
			default: st.push(Integer.parseInt(token));
		}
	}

	return st.pop();
}
```
<u>TIME COMPLEXITY</u>:  O(N)
<u>SPACE COMPLEXITY</u>: O(N)

---
### Online Stock Span
Find the number of consecutive days where the current price of the stock is greater or equal than the previous days.

**Input**
["StockSpanner", "next", "next", "next", "next", "next", "next", "next"]
([], [100], [80], [60], [70], [60], [75], [85])

**Output**
[null, 1, 1, 1, 2, 1, 4, 6]

**Explanation**
StockSpanner stockSpanner = new StockSpanner();
stockSpanner.next(100); // return 1
stockSpanner.next(80);  // return 1
stockSpanner.next(60);  // return 1
stockSpanner.next(70);  // return 2
stockSpanner.next(60);  // return 1
stockSpanner.next(75);  // return 4, because the last 4 prices (including today's price of 75) were less than or equal to today's price.
stockSpanner.next(85);  // return 6

INTUITON: Decreasing monotonic stack - PGE approach but since we don't have the original complete array with the values we need to store on the stack the price and the index.

```java
class StockSpanner {
    private Stack<Pair<Integer, Integer>> st;

    public StockSpanner() {
        st = new Stack<>();    
    }

    public int next(int price) {
        // Stack is empty -> always 1 day
        if (st.isEmpty()){
            st.add(new Pair(price, 0));
            return 1;
        }

        int currentIndex = st.peek().getValue() + 1;
        int count = 1;
        while (!st.isEmpty() && price >= st.peek().getKey()){
            st.pop();
        }

        if (st.isEmpty()){
            count = currentIndex + 1;
        }
        else {
            count = currentIndex - st.peek().getValue();
        }

        st.push(new Pair<>(price, currentIndex));
        return count;
    }

	//
	// OR - Store the span directly instead of the indices
	//

	public int next(int price) {
        int span = 1; // Default span for new price

        // Pop while stack top is smaller/equal and accumulate spans
        while (!st.isEmpty() && price >= st.peek().getKey()) {
            span += st.pop().getValue(); // Accumulate span of popped elements
        }

        st.push(new Pair<>(price, span)); // Store price and its calculated span
        return span;
    }
}
```
<u>TIME COMPLEXITY</u>:  O(N)
<u>SPACE COMPLEXITY</u>: O(N)

---
### Sum of Subarray Minimums
Given an array of integers arr, find the sum of `min(b)`, where `b` ranges over every (contiguous) subarray of `arr`. Since the answer may be large, return the answer **modulo** `109 + 7`.

**Input:** arr = [3,1,2,4]
**Output:** 17
**Explanation:** 
Subarrays are [3], [1], [2], [4], [3,1], [1,2], [2,4], [3,1,2], [1,2,4], [3,1,2,4]. 
Minimums are 3, 1, 2, 4, 1, 1, 2, 1, 1, 1.
Sum is 17.

```java
public static int[] nseCalculation(int[] arr){
	Stack<Integer> st = new Stack<>();
	int n = arr.length;
	int[] nse = new int[n];

	for (int i = n - 1; i >= 0; i--){
		while (!st.isEmpty() && arr[i] < arr[st.peek()]){
			st.pop();
		}

		nse[i] = st.isEmpty() ? n : st.peek();
		st.push(i);
	}
	return nse;
}

public static int[] pseCalculation(int[] arr){
	Stack<Integer> st = new Stack<>();
	int n = arr.length;
	int[] pse = new int[n];  

	for (int i = 0; i < n; i++){
		while (!st.isEmpty() && arr[i] <= arr[st.peek()]){
			st.pop();
		}
		
		pse[i] = st.isEmpty() ? -1 : st.peek();
		st.push(i);
	}
	return pse;
}

public int sumSubarrayMins(int[] arr) {
	int mod = (int)(1e9 + 7);
	int answer = 0;

	int[] nse = nseCalculation(arr);
	int[] pse = pseCalculation(arr);

	for (int i = 0; i < arr.length; i++){
		int left = i - pse[i];
		int right = nse[i] - i;
		answer = ((answer + ((long) right * left * arr[i]) % mod) % mod);
	}

	return (int) answer;
}
```
<u>TIME COMPLEXITY</u>:  O(3N)
<u>SPACE COMPLEXITY</u>: O(4N)

---



---
---
## Prefix, Infix and Postfix Conversions
**Operator:** sum (+), subtraction (-), multiplication (x), division (/), etc.
**Operand:** digits (0-9), letters (a-z & A-Z).

### Infix Notation
The operator is placed between operands (common way to write expressions).

Example: `(A + B) * C`

- Follows operator precedence (multiplication before addition).
- Requires parentheses `()` to explicitly define order of evaluation.
- Needs extra processing to be evaluated using a stack (operator precedence matters).

**Evaluation Using a Stack:** To evaluate infix expressions, we convert them into *Postfix (Reverse Polish Notation - RPN) first.

---
### Postfix Notation
The operator comes after the operands (no need for parentheses).

Example: `AB + C *` which means `(A + B) * C`

- No need for parentheses `()` because the order of operations is implicit.
- Easily evaluated using a single stack (operands are pushed, and operators pop and compute).
- Used in compilers and calculators.

**How to Evaluate Using a Stack?**
1. Scan the expression left to right.
2. Push operands to a stack.
3. When encountering an operator, pop two operands, apply the operation, push the result back.
4. Repeat until the stack has the final result.

---
### Prefix Notation
The operator comes before the operand.

Example: `* + A B C` which means `(A + B) * C`

- No need for parentheses `()`.
- Processed from right to left instead of left to right (opposite of Postfix evaluation).
- Used in some programming languages and mathematical logic systems.

**How to Evaluate Using a Stack?**
1. Scan the expression from right to left.
2. Push operands to a stack.
3. When encountering an operator, pop two operands, apply the operation, push the result back.
4. Repeat until the stack has the final result.

---
## Notation Conversions
### Infix to Postfix
Example: `a + b * (c ^ d - e)`
