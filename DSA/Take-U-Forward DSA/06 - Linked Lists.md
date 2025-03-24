<font color="#7f7f7f"><em>Friday, 01-11-2024 - 15:28</em></font>
#programming #dsa 

## Index
- [[#Theoretical Fundaments]]
- [[#Linked List Insertion, Deletion, Printing (Traversing)]]
- [[#Single Linked List Exercises]]
- [[#Doubly Linked List]]

## Theoretical Fundaments
Linked lists are non-contiguous data structures stored in the heap memory and which size's can be dynamically increased or decreased at any moment. Unlike common arrays that have a static size. 

![[LinkedList Structure | center | 1000]]
Based on the **head** of the linked list we can traverse it, add nodes, remove nodes, search for elements and so on.

##### Where are Linked Lists used?
To implement Stacks, Queues or ArrayLists. Furthermore, they can be akin to a browser tab, clicking on a new link takes you to another page and you can move back and forth between pages.

### Single LL Implementation
Linked Lists are implemented with a custom data structure called Nodes or ListNodes and each node links to another node forming a "Linked List".
```java
class Node {
	public int val;
	public Node next;

	public Node(int val, Node next){
		this.val = val;
		this.next = next;
	}

	public Node(int val){
		this.val = val;
		this.next = null;
	}
}
```

##### Traversing a Linked List
Loop through the linked list and move through the nodes.
`Node current = head`
`while (current != null) => current = current.next`

##### Linked List Insertion, Deletion, Printing (Traversing)
Note that the LinkedList object is just the Node head itself.
```java
class LinkedList {
	private class Node {
		int data;
		Node next;

		public Node(int data){
			this.data = data;
			this.next = null;
		}
	}
	
	private Node head;  // Head (first node) of the linked list

	// -------------------------------
	// ---------- Insertion ----------
	// -------------------------------
	public void insertAtHead(int data){
		Node newNode = new Node(data);

		newNode.next = head;
		head = newNode;
	}

	public void insertAtTail(int data){
		Node newNode = new Node(data);

		if (head == null){
			head = newNode;
			return;
		}

		Node current = head;
		while(current.next != null){
			current = current.next;
		}

		current.next = newNode;
	}

	public void insertAtPosition(int index, int data){
		if (index < 1){ return; }  // Invalid index

		Node newNode = new Node(data);
		
		// CASE 0: Insert at first position
		if (index == 1){
			newNode.next = head;
			head = newNode;
			return;
		}

		// CASE 1: Insert in the middle of the linked list
		Node current = head;
		Node prev = null;
		int count = 0;

		while (current != null){
			count++;
			if (count == index){
				prev.next = newNode;
				newNode.next = current;
				return;
			}

			prev = current;
			current = current.next;
		}

		// CASE 2: Insert at the tail
		if (current == null){
			prev.next = newNode;
		}
	}

	// ------------------------------
	// ---------- Deletion ----------
	// ------------------------------
	public void deleteHead(){
		if (head == null) { return; }  // List is empty
		head = head.next;
	}

	public void deleteTail(){
		if (head == null){ return; }  // List is empty
		if (head.next == null){       // List has 1 node
			head = null;
			return;
		}  

		Node current = head;
		// Loops until the node before the last and deletes the last
		while (current.next.next != null){
			current = current.next;
		}

		current.next = null;
	}

	public void deleteAtPosition(int index){       // 1-index based 
		if (head == null || index < 1){ return; }  // List is empty or index < 1
		if (index == 1){                           // Index is head
			head = head.next;
			return;
		}

		Node current = head;
		Node prev = null;
		int count = 0;

		while (current != null){
			count++;
			
			if (count == index){
				prev.next = current.next;  // Delete node
				return;
			}

			prev = current;
			current = current.next;
		}
	}

	public void deleteByKey(int elem){
		if (head == null){ return; }   // List is empty

		while (head != null && head.data == elem){
			head = head.next;
		}

		Node current = head;
		Node prev = null;
		while (current != null){
			if (current.data == elem){
				prev.next = current.next;  // Remove the node by skipping it
			}
			else {
				prev = current;
			}
			current = current.next;
		}
	}

	// -----------------------------------------
	// ---------- Printing/Traversing ----------
	// -----------------------------------------
	public void printLinkedList(){
		Node current = head;

		while (current != null){
			System.out.println(current.data + " -> ");
			current = current.next;
		}

		System.out.println("null");
	}
}
```

---
---
## Single Linked List Exercises
### Middle of the Linked list
Given the head of a singly linked list, return the middle of the linked list.
If there are two middle nodes, return the second middle node.

##### 1. Brute Force
Traverse the whole entire linked list to check its size and then loop it again until the correct middle node.
```java
public ListNode middleNode(ListNode head) {
	if (head == null){ return null; }
	if (head.next == null){ return head; }

	ListNode current = head;
	int size = 0;
	while (current != null){
		size++;
		current = current.next;
	}

	current = head;
	for (int i = 0; i < size; i++){
		if (i == size / 2){
			return current;
		}

		current = current.next;
	}

	return null;
}
```
<u>TIME COMPLEXITY</u>:  O(3/2 N)
<u>SPACE COMPLEXITY</u>: O(1)

##### 2. Tortoise-Hare Method (Two-Pointer)
Tortoise pointer moves one node at the time while the hare pointer moves two at once. Once the hare pointer reaches the end, the tortoise will be in the middle.
```java
public ListNode middleNode(ListNode head) {
	if (head == null){ return null; }
	if (head.next == null){ return head; }

	ListNode tortoise = head;
	ListNode hare = head.next;

	while (hare != null){
		if (hare.next == null){
			tortoise = tortoise.next;
			break;
		}

		tortoise = tortoise.next;
		hare = hare.next.next;
	}

	return tortoise;
```
<u>TIME COMPLEXITY</u>:  O(N/2)
<u>SPACE COMPLEXITY</u>: O(1)

---
### Delete the middle node of a LL - without cycle
Find the middle node, track the previous of the middle node, remove the middle node in place.

```java
public ListNode deleteMiddle(ListNode head){
	if (head == null || head.next == null){ return null; }

	ListNode prev = null;
	ListNode slow = head;
	ListNode fast = head;

	while (fast != null && fast.next != null){
		prev = slow;
		slow = slow.next;
		fast = fast.next.next;
	}

	prev.next = slow.next;
	return head;
}
```
<u>TIME COMPLEXITY</u>:  O(N/2)
<u>SPACE COMPLEXITY</u>: O(1)

---
### Detect a Cycle in a Linked List
Given head, the head of a linked list, determine if the linked list has a cycle in it.

There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the next pointer.
Internally, pos is used to denote the index of the node that tail's next pointer is connected to. Note that pos is not passed as a parameter.

Return true if there is a cycle in the linked list. Otherwise, return false.

##### 1. Brute Force - HashMap
Map the visited nodes has traversing through the map.
```java
public boolean hasCycle(ListNode head){
	if (head == null || head.next == null){ return false; }

	HashMap<ListNode, Integer> nodeMap = new HashMap<>();
	ListNode current = head;

	while (current != null){
		if (nodeMap.containsKey(current)){
			return true;
		}

		nodeMap.put(current, 1);
		current = current.next;
	}

	return false;
}
```
<u>TIME COMPLEXITY</u>:  O(N)
<u>SPACE COMPLEXITY</u>: O(N)

##### 2. Optimal - Tortoise-Hare (Two-Pointer)
If there's a loop the pointers will eventually meet since the tortoise moves one at the time. If not the hare will reach a null node.
```java
public boolean hasCycle(ListNode head){
	if (head == null || head.next == null){ return false; }

	ListNode slow = head;
	ListNode fast = head.next;

	while (fast != null && fast.next != null){
		if (slow == fast){ return true; }

		slow = slow.next;
		fast = fast.next.next;
	}

	return false;
}
```
<u>TIME COMPLEXITY</u>:  O(N)
<u>SPACE COMPLEXITY</u>: O(1)

---
### Find the starting node of a cycle in a LL
##### 1. Brute Force - HashMap
```java
public ListNode detectCycle(ListNode head) {
	if (head == null || head.next == null) { return null; }

	HashMap<ListNode, Integer> nodeMap = new HashMap<>();
	ListNode current = head;

	while (current != null){
		if (nodeMap.containsKey(current)){
			return current;
		}

		nodeMap.put(current, 1);
		current = current.next;
	}
	return null;
}
```
<u>TIME COMPLEXITY</u>:  O(N)
<u>SPACE COMPLEXITY</u>: O(N)

##### 2. Tortoise-Hare Method (Two-Pointer)
The intuition is to use the tortoise-hare method and after the pointers meet for the first time, change one back to the head and change the speed of the fast one to be the same as the slow. Once they meet again, you've reached the start of the cycle.

```java
    public ListNode detectCycle(ListNode head) {
        if (head == null || head.next == null){ return null; }

        ListNode slow = head;
        ListNode fast = head;
        while (fast != null && fast.next != null){
            slow = slow.next;
            fast = fast.next.next;

            if (slow == fast){
                fast = head;
                break;
            }
        }
  
        if (fast == null || fast.next == null){  // Just in case there's no cycle
            return null;
        }

        while (fast != slow){
            slow = slow.next;
            fast = fast.next;
        }
        return slow;
    }
```
<u>TIME COMPLEXITY</u>:  O(2N)
<u>SPACE COMPLEXITY</u>: O(1)

---
### Length of the cycle in the LL
Same concept as finding the start of the cycle. After the fast and slow pointers meet for the first time, reset one, change the speed of the fast to be slow and count the number of iterations until the meet again.

<u>TIME COMPLEXITY</u>:  O(2N)
<u>SPACE COMPLEXITY</u>: O(1)

---
### Remove Nth Node from end of list
The intuition is to move the fast pointer n steps ahead and then start moving both pointers step by step, by the end when the fast pointer reaches null the slow pointer will be on the N-th node since we've made a gap/distance of n between these two.

 4      3      2      1  -> positions (n)
|1| -> |2| -> |3| -> |4| -> null

```java
public ListNode removeNthFromEnd(ListNode head, int n){
	if (head == null || n < 1){ return null; }
	if (head.next == null && n == 1 ){ return null; }

	ListNode fast = head;
	for (int i = 0; i < n; i++){
		fast = fast.next;
	}

	// Remove head
	if (fast == null){ return head.next; }

	ListNode slow = head;
	ListNode prev = slow;
	while (fast != null){
		prev = slow;
		slow = slow.next;
		fast = fast.next;
	}

	prev.next = slow.next;
	return head;
}
```
<u>TIME COMPLEXITY</u>:  O(N)
<u>SPACE COMPLEXITY</u>: O(1)

---
### Reverse Linked List
Given the head of a singly linked list, reverse the list, and return the reversed list (head).

##### 1. Brute Force - Extra LL
Traverse the linked list and add the nodes to the head of the new reversed linked list.
```java
public static ListNode insertAtHead(ListNode head, int data){
	ListNode newNode = new ListNode(data);

	newNode.next = head;
	head = newNode;
	return head;
}

public ListNode reverseList(ListNode head) {
	if (head == null){ return null; }

	ListNode reverseHead = new ListNode(head.val);
	ListNode current = head.next;
	ListNode prev = head;
	while(current != null){
		reverseHead = insertAtHead(reverseHead, current.val);

		prev = current;
		current = current.next;
	}
	return reverseHead;
}
```
<u>TIME COMPLEXITY</u>:  O(N)
<u>SPACE COMPLEXITY</u>: O(N)

##### 2. Reverse In-Place
```java
public ListNode reverseList(ListNode head) {
	if (head == null){ return null; }
	if (head.next == null){ return head; }

	Node current = head;  // Temp will be the current
	Node prev = null;

	while (current != null){
		Node front = current.next;  // Store the front node to preserve the link
		current.next = prev;        // Change the current link to one behind

		prev = current;             // Move the prev to the next node
		current = front;            // Move the current to the next node
	}

	return prev;
}
```
<u>TIME COMPLEXITY</u>:  O(N)
<u>SPACE COMPLEXITY</u>: O(1)

##### 3. Optimal - Recursion (???)
```java
public ListNode reverseList(ListNode head) {
	// Recursion base case
	if (head == null || head.next == null){ return head; }

	ListNode newHead = reverseList(head.next);
	ListNode front = head.next;

	front.next = head;
	head.next = null;

	return newHead;
}
```
<u>TIME COMPLEXITY</u>:  O(N)
<u>SPACE COMPLEXITY</u>: O(1)

---
### Sort a linked list
##### 1. Brute Force - LL to Array to LL
```java
public ListNode sortList(ListNode head) {
	if (head == null || head.next == null){ return head; }

	ArrayList<Integer> arr = new ArrayList<>();
	ListNode current = head;
	while (current != null){
		arr.add(current.data);
		current = current.next;
	

	Collections.sort(arr);

	// Create a new LL
	ListNode sortedHead = new Node(arr.get(0));
	current = sortedHead;
	for (int i = 1; i < arr.size(); i++){
		ListNode newNode = new ListNode(arr.get(i));
		current.next = newNode;
		current = current.next;
	}

	return sortedHead;

	//
	// OR 
	//
	
	// Use the same LL
	ListNode temp = head;
	int i = 0;
	while (temp != null){
		temp.data = arr.get(i);
		temp = temp.next;
		i++;
	}
	return head;
}
```
<u>TIME COMPLEXITY</u>:  O(2N + NLOG N)
<u>SPACE COMPLEXITY</u>: O(N)

##### 2. Merge Sort Approach - O(NLOG N)
```java
public ListNode mergeSortList(ListNode list1, ListNode list2){
	ListNode sortedHead = new ListNode();  // Null value and next node
	ListNode current = sortedHead;

	while (list1 != null && list2 != null){
		if (list1.data <= list2.data){
			current.next = list1;  // makes the link from current (new sorted list) to list1 node
            list1 = list1.next;    // move list1 node to the next
		}
		else {
			current.next = list2;
			list2 = list2.next;
		}
		
		// move current to the next that has been updated in the if-else already
		current = current.next;    
	}
	
	// If there is nodes left in the lists append them
	// note that only needs to update current.next to the list head at that point
	// unlike array merge sort that we had to traverse the left array
	if (list1 != null){
		current.next = list1;
	}
	else {
		current.next = list2;
	}

	return sortedHead.next;
}


public ListNode findMiddle(ListNode head){
	if (head == null || head.next == null){ return head; }

	ListNode slow = head;
	ListNode fast = head.next;  // IMPORTANT: if fast = head we'll get the wrong middle
	while (fast != null && fast.next != null){
		slow = slow.next;
		fast = fast.next.next;
	}

	return slow;
}


public ListNode sortList(ListNode head) {
	if (head == null || head.next == null){ return head; }

	ListNode middle = findMiddle(head);
	ListNode left = head;
	ListNode right = middle.next;

	middle.next = null;  // IMPORTANT; Breaks the lists by adding null to the next node

	left = sortList(left);
	right = sortList(right);
	
	return mergeSortList(left, right);
}
```
<u>TIME COMPLEXITY</u>:  O(NLOG N)
<u>SPACE COMPLEXITY</u>: O(1)

---
### Palindrome Linked List
Intuition is find the middle, reverse in-place the middle onwards and then compare the list starting on head with the new middle onwards reversed list.

```java
public boolean isPalindrome(ListNode head) {
	if (head == null || head.next == null){ return true; }
	
	ListNode slow = head;
	ListNode fast = head;
	while (fast != null && fast.next != null){  // find the middle = slow
		slow = slow.next;
		fast = fast.next.next;
	}

	ListNode prev = null;
	ListNode current = slow;
	while (current != null){  // reverse right middle side in-place
		ListNode nextNode = current.next;
		current.next = prev;
		prev = current;
		current = nextNode;
	}
	
	ListNode first = head, second = prev;
	// Traverse starting on the right middle side until the end
	// If you traverse from head != null eventually the second will be null
	// while the head will still have the middle to traverse - nullpointerexcep
	while (second != null){  
		if (first.val != second.val){ return false; }
		first = first.next;
		second = second.next;
	}
	return true;
}
```
<u>TIME COMPLEXITY</u>:  O(N) - O(N/2 + N/2 + N/2 = 3/2N)
<u>SPACE COMPLEXITY</u>: O(1)

---
### Add two numbers
Note that there is the constraint that nodes can hold values higher than 9, so you need to carry the remainder to the next node summation.

##### 1. Messy Implementation
```java
public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
	ListNode head = new ListNode();
	ListNode current = head;
	int carry = 0;
	int sum = 0;

	while (l1 != null && l2 != null){
		sum = l1.val + l2.val + carry;
		int digit = sum;
		
		if (sum > 9){
			digit = sum % 10;
			carry = sum / 10;
		}
		else {
			carry = 0;
		}

		ListNode newNode = new ListNode(digit);
		current.next = newNode;
		current = current.next;
		l1 = l1.next;
		l2 = l2.next;
	}

	while (l1 != null){
		sum = l1.val + carry;
		int digit = sum;

		if (sum > 9){
			digit = sum % 10;
			carry = sum / 10;
		}
		else {
			carry = 0;
		}

		ListNode newNode = new ListNode(digit);
		current.next = newNode;
		current = current.next;
		l1 = l1.next;
	}

	while (l2 != null){
		sum = l2.val + carry;
		int digit = sum;
		
		if (sum > 9){
			digit = sum % 10;
			carry = sum / 10;
		}
		else {
			carry = 0;
		}
		
		ListNode newNode = new ListNode(digit);
		current.next = newNode;
		current = current.next;
		l2 = l2.next;
	}

	if (carry != 0){
		ListNode newNode = new ListNode(carry);
		current.next = newNode;
		current = current.next;
	}
	return head.next;
}
```
<u>TIME COMPLEXITY</u>:  O(max(N,M))
<u>SPACE COMPLEXITY</u>: O(max(N,M))

##### 2. Not Messy Implementation
```java
public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
	ListNode head = new ListNode();
	ListNode current = head;
	int carry = 0;

	while (l1 != null || l2 != null || carry > 0){
		int sum = carry;

		if (l1 != null){
			sum += l1.val;
			l1 = l1.next;
		}
		if (l2 != null){
			sum += l2.val;
			l2 = l2.next;
		}

		carry = sum / 10;  // Always calculate it, it will be either 0 or 1 - if 0 it won't interefere
		ListNode newNode = new ListNode(sum % 10);
		current.next = newNode;
		current = current.next;
	}
	
	return head.next;
}
```
<u>TIME COMPLEXITY</u>:  O(max(N,M))
<u>SPACE COMPLEXITY</u>: O(max(N,M))

---
### Odd Even Linked List - Changing Links
Group the odd node values at the start and only then all the even node values. 
Intuition: traverse through the list with two pointers and update the links to odd/even nodes correctly, at the end link the odd link with the head of the even node list.

```java
public ListNode oddEvenList(ListNode head) {
	if (head == null || head.next == null) return head;

	ListNode oddHead = head, evenHead = head.next;
	ListNode currentOdd = oddHead, currentEven = evenHead;

	// Rearrange odd and even indexed nodes
	while (currentEven != null && currentEven.next != null) {
		currentOdd.next = currentEven.next;  // Connect odd nodes
		currentOdd = currentOdd.next;        // Move odd pointer

		currentEven.next = currentOdd.next;  // Connect even nodes
		currentEven = currentEven.next;      // Move even pointer
	}

	// Attach even list to the end of odd list
	currentOdd.next = evenHead;  

	return head;
}
```
<u>TIME COMPLEXITY</u>:  O(N)
<u>SPACE COMPLEXITY</u>: O(1)

---
---
## Doubly Linked List
