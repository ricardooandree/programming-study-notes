<font color="#7f7f7f"><em>Tuesday, 04-03-2025 - 22:18</em></font>
#programming #oop #io-operations 

>[!info] 
>This note covers Java’s basic input/output operations: reading from and writing to the console, working with files, handling resources safely, and using buffered and formatted I/O.

## ✳️ Console Input/Reading

Use `Scanner` object to get user input from the terminal (standard input).

```java
import java.util.Scanner;

Scanner scanner = new Scanner(System.in);

System.out.print("Enter your name: ");
String name = scanner.nextLine();

System.out.println("Hello, " + name);

scanner.close();
```

| Scanner Methods  | Description                  |
| ---------------- | ---------------------------- |
| `next()`         | Reads next token             |
| `nextLine()`     | Reads full line (String)     |
| `nextInt()`      | Reads integer                |
| `nextDouble()`   | Reads decimal number         |
| `hasNextInt()`   | Checks if next input is int  |
| `hasNext()`      | Checks if there's more input |
| `useDelimiter()` | Set custom token separator   |
### Practical Input Scenarios

##### Read Until a Condition is Met (`-1`)
```java
Scanner scanner = new Scanner(System.in);

int input;

System.out.println("Enter numbers (-1 to stop): ");

while ((input = scanner.nextInt()) != -1){
	System.our.println("You entered: " + input);
}

scanner.close();
```

##### Split a Full Name into First/Last
```java
Scanner scanner = new Scanner(System.in);

System.out.print("Enter full name: ");

String fullName = scanner.nextLine();
String[] parts = fullName.split(" ");
String firstName = parts[0];
String lastName = parts.length > 1 ? parts[1] : "";

System.out.println("First: " + firstName + " , Last: "+ lastName);
scanner.close();
```

##### Split a Line of Comma-Separated Values
```java
Scanner scanner = new Scanner(System.in);
System.out.print("Enter values (comma-separated): ");
String line = scanner.nextLine();

String[] values = line.split(",");

for (String value : values) {
	System.out.println("Value: " + value.trim());
}
scanner.close();
```

##### Converting String to Integer/Double
```java
Scanner scanner = new Scanner(System.in);

System.out.print("Enter age: ");
String input = scanner.nextLine();

int age = Integer.valueOf(input);  // or Integer.parseInt(input)
System.out.println("Age: " + age);
scanner.close();
```

>[!important]
>If the input is not a valid number, this will throw `NumberFormatException`.

---
## ✳️ Console Output

Use `System.out` for standard output.

```java
System.out.print("Hello");    // No newline
System.out.println("World");  // Adds newline
```

> Use `System.err.println()` for error messages (prints to error stream).

---
## ✳️ File Input/Output (Text Files)

### Writing to Files - `FileWriter` and `BufferedWriter`

```java
import java.io.*;

BufferedWriter writer = new BufferedWriter(new FileWriter("output.txt"));
writer.write("Hello, file!");
writer.newLine();
writer.close();
```

### Reading from Files - `BufferedReader`

```java
import java.io.*;

try (BufferedReader reader = new BufferedReader(new FileReader("data.txt"))) {
	String line;
	while ((line = reader.readLine()) != null){
		System.out.println(line);
	}
	
} catch (IOException e) {
	System.err.println("Error reading file: " + e.getMessage());
	
}
```

> Try-Catch closes streams after use -> works with any class implementing `AutoCloseable`.

---
## ✳️ File Object - `File`

Use the `File` class to inspect and manipulate files/directories.

```java
File file = new File("data.txt");

if (file.exists()){
	System.out.println("Name: " + file.getName());
	System.out.println("Path: " + file.getAbsolutePath());
	System.out.println("Size: " + file.length());
}
```

> `createNewFile()` is another File method that creates a new empty file.

---
## ✳️ Writing with PrintWriter (Formatted Output)

```java
import java.io.*;

PrintWriter writer = new PrintWriter("report.txt");

writer.printf("Score: %d%%\n", 95);
writer.close();
```

> `printf()` supports C-style formatting (`%d`, `%f`, `%s`, etc.)

---
## ✳️ Common I/O Exceptions

| Exception               | Cause                                 |
| ----------------------- | ------------------------------------- |
| `FileNotFoundException` | File does not exist for reading       |
| `IOException`           | General I/O failure (e.g. disk error) |
| `SecurityException`     | No permission to access file          |
| `EOFException`          | Unexpected end of file                |

>File operations often require **exception handling** => use try-catch or propagate with `throws`.

---
## ✳️ Best Practices for I/O

>[!list] Best Practices for I/O
>- Use `try-with-resources` to auto-close streams
>- Prefer BufferedReader/Writer over unbuffered classes for performance
>- Catch and log `IOException` properly
>- Check `file.exists()` before reading/deleting
>- Avoid hardcoded paths - use relative paths or config files

