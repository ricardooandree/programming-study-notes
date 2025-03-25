<font color="#7f7f7f"><em>Wednesday, 11-09-2024 - 15:37</em></font>
#programming #oop #exception-handling

>[!info]
>This note covers Java’s error handling system, focusing on exceptions—how to throw, catch, propagate, and define them clearly and safely using best practices.

## ✳️ What is an Exception?

An **exception** is an event that disrupts the normal flow of a program. It signals that something unexpected happened during execution.  
Java allows handling such events gracefully instead of crashing the program.

## ✳️ Check vs. Unchecked Exceptions

### Checked Exceptions

**Checked exceptions** are exceptions that the compiler forces you to handle—either by catching them with a `try-catch` block or declaring them with `throws`. They usually represent recoverable issues, like missing files or bad input.

- Must be declared with `throws` or caught in a `try-catch` block.
- Represent **recoverable issues** (e.g. file not found, invalid input).
- Extend `Exception` but not `RuntimeException`.

```java
public void readFile() throws IOException {
	BufferedReader reader = new BufferedReader(new FileReader("file.txt"));
}
```

### Unchecked Exceptions

**Unchecked exceptions** (which include `RuntimeException` and its subclasses) don’t have to be handled at compile time. They usually represent bugs in the program, like logic errors (e.g., null pointer or out-of-bounds access), and are only caught at **runtime**.

- Do not require handling at compile time - Not required to be declared or caught.
- Represent programming errors (e.g. null pointers, index out of bounds).
- Extend `RuntimeException`.

```java
public int divide(int a, int b){
	if (b == 0) throw new ArithmeticException("Cannot divide by zero");

	return a / b;
}
```

> **`RuntimeException`** is a special class in Java that serves as the parent for most unchecked exceptions. Any exception that extends `RuntimeException` is considered unchecked.

> **Compile time:** is when the code is being translated to bytecode (before running)
> **Runtime** is when the program is actually executing.

## ✳️ Try-Catch-Finally Blocks

These are special code blocks used to catch exceptions and define alternate control flow.

```java
try {
	int result = 10 / 0;
	
} catch (ArithmeticException e) {
	System.out.println("Error: " + e.getMessage());
	
} finally {
	System.out.println("This block always runs");
}
```

**Flow:**
1. `try` - code that might throw an exception
2. `catch` - handles specific exception types
3. `finally` - runs regardless of outcome (optional)

>[!tip]
>Use `finally` to close resources or clean up (e.g. closing files, streams or DB connections).

## ✳️ Throwing Exceptions - `throw`

Use `throw` to **manually** trigger an exception - custom exceptions too.

```java
if (age < 0){
	throw new IllegalArgumentException("Age cannot be negative");
}
```

> You can only `throw` instances of Throwable (e.g. `Exception`, `Error`).

## ✳️ Propagating Exceptions - `throws`

Use `throws` to declare that a method *might* throw an exception, pushing the responsibility to the caller.

```java
public void readData() throws IOException {
	// this method doesn't catch it; caller must handle it
}
```

>[!important]
>Use `throws` when the method can't or shouldn't handle the error (e.g. library methods, I/O operations).
>Use `try-catch` when you can recover or log immediately (e.g. fallback logic, user interaction).

## ✳️ Creating Custom Exceptions

You can define your own exceptions by extending `Exception` or `RuntimeException`.

```java
public class InvalidAgeException extends Exception {
	public InvalidAgeException(String message){
		super(message);
	}
}

// Usage
public void setAge(int age) throws InvalidAgeException {
	if (age < 0) {
		throw new InvalidAgeException("Age must be positive");
	}
}
```

>[!important] 
>This is particularly useful for when we have a **Global Exception Handlers** - often in backend works (Java + Spring) - This is a way of catching and managing exceptions **centrally**, instead of wrapping try-catch blocks around every method.

## ✳️ Best Practices for Error Handling & Exceptions Management

>[!list] Best Practices for Error Handling & Exceptions Management
>- Catch the most specific exceptions first
>- Always log or handle the exception meaningfully
>- Avoid empty `catch` blocks
>- Use custom exceptions for domain-specific errors
>- Prefer unchecked exceptions only for programming errors


