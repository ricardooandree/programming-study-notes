<font color="#7f7f7f"><em>Friday, 01-11-2024 - 15:32</em></font>

## Index


## Theoretical Fundaments


## Exercises
### Assign Cookies

**Input:** g = [1,2,3], s = [1,1]
**Output:** 1
**Explanation:** You have 3 children and 2 cookies. The greed factors of 3 children are 1, 2, 3. 
And even though you have 2 cookies, since their size is both 1, you could only make the child whose greed factor is 1 content.
You need to output 1.

**Input:** g = [1,2], s = [1,2,3]
**Output:** 2
**Explanation:** You have 2 children and 3 cookies. The greed factors of 2 children are 1, 2. 
You have 3 cookies and their sizes are big enough to gratify all of the children, 
You need to output 2.

INTUITON: Sort the arrays and then we can traverse both arrays being sorted. At this point it means that the lowest greed child is at the start of the array and the smallest cookie too. While traversing the cookies array if the current cookie size >= current child's greed then that child is content.

```java
public int findContentChildren(int[] g, int[] s) {
	if (g.length == 0 || s.length == 0){ return 0; }

	Arrays.sort(g);
	Arrays.sort(s);

	int childPtr = 0, cookiePtr = 0;
	int count = 0;

	while (childPtr < g.length && cookiePtr < s.length){
		if (s[cookiePtr] >= g[childPtr]){
			count++;
			childPtr++;
		}
		
		cookiePtr++;
	}

	return count;
}
```
<u>TIME COMPLEXITY</u>:  O(NLOG N + MLOG M + N)
<u>SPACE COMPLEXITY</u>: O(1)

---
### Lemonade Change
At a lemonade stand, each lemonade costs `$5`. Customers are standing in a queue to buy from you and order one at a time (in the order specified by bills). Each customer will only buy one lemonade and pay with either a `$5`, `$10`, or `$20` bill. You must provide the correct change to each customer so that the net transaction is that the customer pays `$5`.

Note that you do not have any change in hand at first.

Given an integer array `bills` where `bills[i]` is the bill the `ith` customer pays, return `true` _if you can provide every customer with the correct change, or_ `false` _otherwise_.

**Input:** bills = [5,5,5,10,20]
**Output:** true
**Explanation:** 
From the first 3 customers, we collect three $5 bills in order.
From the fourth customer, we collect a $10 bill and give back a $5.
From the fifth customer, we give a $10 bill and a $5 bill.
Since all customers got correct change, we output true.

**Input:** bills = [5,5,10,10,20]
**Output:** false
**Explanation:** 
From the first two customers in order, we collect two $5 bills.
For the next two customers in order, we collect a $10 bill and give back a $5 bill.
For the last customer, we can not give the change of $15 back because we only have two $10 bills.
Since not every customer received the correct change, the answer is false.

INTUITION: There's only a two-case for when you get a $20 bill and a one case for a $10 bill, just a matter of keeping track of the bills with counters.

```java
public boolean lemonadeChange(int[] bills) {
	int five = 0, ten = 0;
	
	for (int bill : bills) {
		if (bill == 5) {
			five++;

		} else if (bill == 10) {
			if (five > 0){
				five--;
				ten++;
			}
			else return false;

		} else if (bill == 20) {
			if (five > 0 && ten > 0){
				five--;
				ten--;
			}
			else if (five >= 3){
				five -= 3;
			}
			else return false;
		}
	}

	return true;
}
```
<u>TIME COMPLEXITY</u>:  O(N)
<u>SPACE COMPLEXITY</u>: O(1)

---
### Valid Parenthesis String
Given a string `s` containing only three types of characters: `'('`, `')'` and `'*'`, return `true` _if_ `s` _is **valid**_.

The following rules define a **valid** string:
- Any left parenthesis `'('` must have a corresponding right parenthesis `')'`.
- Any right parenthesis `')'` must have a corresponding left parenthesis `'('`.
- Left parenthesis `'('` must go before the corresponding right parenthesis `')'`.
- `'*'` could be treated as a single right parenthesis `')'` or a single left parenthesis `'('` or an empty string `""`.

**Input:** s = "()"
**Output:** true

**Input:** s = "(*)"
**Output:** true

**Input:** s = "(*))"
**Output:** true

NOTE: Parenthesis have to match so `)(` is not valid.

INTUITION: Iterate forward through the string, count the number of open and star characters, if you encounter a closing bracket then you need to match it, so check if open > 0 (first priority) and if not check if you have stars to match (second priority), if you don't have any then that closing bracket is unmatched. 
After this, reset the number of stars and iterate the string backwards, do the same but try to match opening brackets. At the end if both loops are finished it means you always managed to match closing and opening brackets. 

NOTE: There's a 2 counter min/max approach and a double stack approach and a counter + recursion apprach.

```java
public boolean checkValidString(String s){
	int open = 0, close = 0, stars = 0;
	
	for (char c : s.toCharArray()){
		if (c == '('){
			open++;
		}
		else if (c == '*'){
			stars++;
		}
		else {
			if (open == 0){
				if (stars == 0){
					return false;
				}
				else {
					stars--;
				}
			}
			else {
				open--;
			}
		}
	}

	if (open == close) { return true; }
	
	stars = 0;
	for (int i = s.length() - 1; i >= 0; i--){
		char c = s.charAt(i);
	
		if (c == ')'){}
			close++;
		}
		else if (c == '*'){
			stars++;
		}
		else {
			if (close == 0){
				if (stars == 0){
					return false;
				}
				else {
					stars--;
				}
			}
			else {
				close--;
			}
		}
	}
	
	return true;
}
```
<u>TIME COMPLEXITY</u>:  O(2N)
<u>SPACE COMPLEXITY</u>: O(1)

---
