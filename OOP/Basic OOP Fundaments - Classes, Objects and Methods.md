<font color="#7f7f7f"><em>Tuesday, 04-03-2025 - 21:58</em></font>

>[!info] 
>This note covers foundational concepts in object-oriented programming related to class and object structure, method behavior, access control, static members, base methods, data types, and modern Java features like generics and lambdas.


## ✳️ Classes & Objects

### What is a Class?

A **class** is a blueprint for creating objects. It defines the properties (fields) and behaviours (methods) common to all instances of that type.

```java
public class Car {
	String model;
	int year;

	void drive(){
		System.out.println("Driving...");
	}
}
```

### What is an Object?

An **object** is an instance of a class. It occupies memory and can invoke methods defined in its class.

```java
Car myCar = new Car();
myCar.drive();
```

---
## ✳️ Constructors and Destructors

### What is a Constructor?

**Constructors** are special methods that initialize objects. These much match the class name and can be **overloaded** (multiple constructors with different parameters).

```java
public class Person {
	String name;
	int age;

	public Person(String name, int age){
		this.name = name;
		this.age = age;
	}
}
```

### Constructor Chaining with `this()`

```java
public class Person {
	String name;
	int age;

	public Person(String name) {
		this(name, 0); // calls next constructor
	}

	public Person(String name, int age) {
		this.name = name;
		this.age = age;
	}
}
```

### Private Constructors

A **private constructor** is a constructor that cannot be accessed from outside the class.

**Why use private constructors?**
- Prevent instantiation of utility/helper classes (`Collections`, `Math`)
- Enforce singleton pattern
- Control object creation (e.g. use factory methods)

```java
// Singleton Class
public class Singleton {
	private static final Singleton instance = new Singleton();

	private Singleton() {}  // Prevents external instantiation

	public static Singleton getInstance() {
		return instance;
	}
}

// Utility classes
public class MathUtils {
	private MathUtils() {}  // Cannot be instantiated

	public static int square(int x) {
		return x * x;
	}
}
```

>[!important]
>Private constructors still allow instantiation **from within the class itself**. This is how singletons and factories work!

### What is a Destructor?

**Destructors** are methods to destroy objects => release the memory associated with them.
Java doesn't have destructors in the traditional sense since memory is managed via the **Garbage Collector** -> [[Object Lifecycle & Memory Management]].
The `finalize()` method was historically used for clean-up but is now **deprecated** and should be avoided.

---
## ✳️ Instance vs. Static Members

### What is a Member?

A **member** is any variable or method that belongs to a class. Members can be **instance-level** or **static**.

### Types of Members

**Instance Members:** Belong to each object individually and are accessed via the object's reference.

```java
Car c = new Car();
c.model = "Tesla";  // Model is an instance member
```

**Static Members:** Belong to the class itself thus are shared across all instances (objects) and can be accesses via `ClassName.member`.

```java
public class MathUtils {
	static int square(int x){
		return x * x;
	}
}
```

>[!important] 
>Static members can be used without creating an object.


**Static Blocks:** Used for static initialization => are executed **once** when the class is first loaded.

```java
static {
	System.out.println("Class loaded!");
}
```

### Initialization Order
1. Static variables and static blocks (in order)
2. Instance variables and blocks
3. Constructor

### Why Static Methods Cannot Be Overridden?

Static methods are resolved at compile time (no dynamic dispatch). If a subclass declares a static method with the same signature, it **hides** the parent method - it doesn't override it.

---
## ✳️ Access Modifiers

Access modifiers control visibility of classes, attributes, and methods.

| Modifier    | Same Class | Same Package | Subclass | Others |
| ----------- | :--------: | :----------: | :------: | :----: |
| `public`    |     ✅      |      ✅       |    ✅     |   ✅    |
| `protected` |     ✅      |      ✅       |    ✅     |   ❌    |
| default     |     ✅      |      ✅       |    ❌     |   ❌    |
| `private`   |     ✅      |      ❌       |    ❌     |   ❌    |

---
## ✳️ Getters and Setters

Getters and setters are **accessor methods** used to read/write private fields. They're a core part of **encapsulation**.

```java
public class Product {
	private String name;

	public String getName(){ return name; }
	public void setName(String name){ this.name = name; }
}
```

---
## ✳️ Base Object Methods (Equals, HashCode, ToString)

Every class in Java inherits from `Object`, which provides the following methods:

### `toString()` 

Returns a string representation of the object. Override to customize.

```java
@Override
public String toString(){
	return name + " (" + age + ")";
}
```

### `equals(Object o)`

Tests if objects are **logically equal** (not just same memory).

```java
@Override
public boolean equals(Object o){
	if (!(o instanceof Person)) return false;

	Person p = (Person) o;
	return this.name.equals(p.name);  // Checks if the object o name is == to this
}
```

### `hashCode()`

Used in hash-based collections (e.g. `HashMap`) and should be consistent with `equals()`. Serves as a unique identifier to an object.

```java
@Override
public int hashCode(){
	return Objects.hash(name, age);
}
```

> Always override `equals()` and `hashCode()` together!

>[!important]
>== checks memory addresses. Use `equals()` for content comparison.
>If two objects are equal, they must return the same hash code.

---
## ✳️ Data Types & Generics, Value & Reference

### Primitive Data Types

A **primitive** is a basic, low-level data type that **holds actual values** directly. Thus, is stored directly in the **stack** memory.

- Hold actual values
- Stored in **stack memory**
- No methods

```java
int a = 5;
double x = 2.5;
boolean ok = true;
```

### Reference Data Types

A **reference** type doesn’t hold the actual value — it **holds a reference (memory address)** that points to an object in the **heap** and since its an object it can access object's members via this reference.

- Hold memory addresses pointing to objects on the **heap** memory
- Can access object members
- Can be `null`

```java
String name = "Alice";         
int[] numbers = {1, 2, 3};     
Book myBook = new Book();      
```

### Wrapper Classes

Box primitives into objects (`Integer`, `Double`, `Boolean`) so they can be used in collections or generic types - utility methods.

### Generic Data Types

A non-defined data type that allows type-safety to classes and methods.

```java
public class Box<T> {
	T item;

	public void set(T item){ this.item = item; }
	public T get(){ return item; }
}
```

### Value vs. Reference in Java

Java is always **pass-by-value** - even with objects.

- For **primitives**, the actual value is copied.
- For **objects**, the **reference is copied**, meaning the original object can still be modified, but the reference itself cannot be reassigned from inside the method.

```java
void modify(Person p){
	p.setName("Ana");       // This affects the original object - modification
	p = new Person("Bob");  // This has no effect outside - re-assigning
}
```

---
## ✳️ Lambda Expressions & Functional Style

### What is a Lambda Expression?

A concise way to define **anonymous functions**. Used mainly with functional interfaces.

**Syntax:** (parameter1, parameter2) -> { body }

```java
// Declaring a sum object that's initialized through a lambda expression
Integer sum = (x, y) -> x + y;
```

```java
public int addition(int x, int y){
	return x + y;
}
```

### What is a Method Reference?

A **method reference** is a shortcut to refer to an existing method by name, using `::`. (A shorthand for lambdas that call a single method).

```java
List<String> names = Arrays.asList("Alice", "Bob", "Charlie");

// Lambda
names.forEach(name -> System.out.println(name));

// Method reference
names.forEach(System.out::println);

```

---
## ✳️ Streams

A **stream** in Java is a sequence of elements that supports functional-style operations for processing collections of data in a declarative, pipeline-based way.

Streams **DO NOT** modify the original data source.

```java
List<String> names = Arrays.asList("Alice", "Bob", "Charlie");

names.stream()  // creates a stream from the list
     .filter(name -> name.startsWith("A"))  // keeps names that start with "A"
     .map(String::toUpperCase)  // converts to uppercase
     .forEach(System.out::println);  // prints each result
```

> Use `collect(Collectors.toList())` to build results into a new collection after a stream operation.

Streams Key Features:
- Works with **collections**
- Uses **functional interfaces**
- Can be **chained**
- Supports **lazy evaluation**
- Has **parallel** versions (`parallelStream()`)
