<font color="#7f7f7f"><em>Tuesday, 04-03-2025 - 22:06</em></font>
#programming #oop #memory 

>[!info] 
>This note explains how objects are created, stored in memory, and eventually cleaned up by Java’s garbage collector. It also touches on copying/cloning, and the differences between stack and heap memory.

## ✳️ Object Creation – `new` Keyword

Objects in Java are created using the `new` keyword, which allocates memory on the heap and calls the class constructor.

```java
Person p = new Person("Alice");
```

```java
Person p;  // This is a reference variable - Stack created which later can
		   // point to an object 
```

> All Java objects are stored in the **heap** memory regardless of where they're declared.

---
## ✳️ Stack vs. Heap Memory

**Stack memory**: Memory used for storing method calls and local variables, managed in a LIFO (last-in, first-out) way.

**Heap memory**: Memory used for storing objects and class instances, accessible globally and managed by the garbage collector.

| Feature      | Stack                         | Heap                            |
| ------------ | ----------------------------- | ------------------------------- |
| Stores       | Local variables, method calls | Objects, class instances        |
| Lifetime     | Short-lived (per method call) | Long-lived (until GC cleans it) |
| Managed by   | JVM call stack                | JVM + Garbage Collector         |
| Access Speed | Very fast                     | Slower than stack               |
| Thread-safe  | Yes (each thread has its own) | No (shared across threads)      |

---
## ✳️ Garbage Collector (GC)

Java automatically releases/deallocated memory for objects that are no **longer referenced**. This process is called **Garbage Collection** and is done automatically by the JVM, based on **memory pressure** and **object reachability**.

>You don't need to explicitly free memory (like in C/C++).

### How Garbage Collectors Work (High-Level)
##### Object Reachability

Refers to whether an object is **accessible** from any live part of the application. If no references can reach an object, it becomes **unreachable** and is eligible for GC.

```java
Person p = new Person("Alice");  // reachable
p = null;                        // now unreachable
```

```java
Person p1 = new Person("Bob");
Person p2 = p1;                  
p1 = null                        // still reachable via p2
```

```java
public class Demo {
	public static void main(String[] args){
		createPerson();  // After this call, the Person object is unreachable 
		System.gc();     // Suggest garbage collection
	}

	public static void createPerson(){
		Person p = new Person("Eve");  // p is only valid inside this method
	}
}
```

> The GC identifies unreachable objects (no path from active variables, threads, or stack frames).

##### Memory Pressure

Memory pressure means that the **heap is getting full**, and the JVM is struggling to allocate space for new objects. This typically **triggers garbage collection** to free up space.

```java
List<byte[]> list = new ArrayList<>();

while (true){
	list.add(new byte[1_000_000]);  // Keep allocation 1MB chunks - OutOfMemory
}
```

> This example causes an `OutOfMemoryError` if the GC can't free up space fast enough.

>[!important]
>You can also suggest GC with `System.gc()` but it's not guaranteed.
>The `finalize()` method (once used for clean-up) is **deprecated**.

### How They're Implemented

Java uses different **GC algorithms**, like:
- **Serial GC** - (stop-the-world, single-threaded)
- **Parallel GC** - (multi-threaded, default for many apps)
- **G1 GC** - (splits heap into regions for faster clean-up)
- **ZGC / Shenandoah** - (low-latency, scalable GCs)

---
## ✳️ Object Copying - Shallow vs. Deep

**Shallow Copy:** Copies references, **not the inner objects**. Both variables point to the same object - changes to one affect the other.

```java
Person original = new Person("Alice", new Address("NY"));
Person copy = original;  // both refer to the same object
```

**Deep Copy:** Creates a **new object** and copies everything recursively - including nested objects.

```java
Person deepCopy = new Person(original.getName(), new Address(original.getAddress().getCity()));
```

>Deep copies are safer but require more code.

---
## ✳️ Object Cloning - `clone()` Method

You can override the `clone()` method from the `Object` class to copy objects - similar to how you can do it with `equals()`.

```java
public class Person implements Cloneable {
	private String name;

	@Override
	public Person clone() {
		try {
			return (Person) super.clone();
			
		} catch (CloneNotSupportedException e){
			throw new RuntimeException(e);
		}
	}
}
```

>[!important]
>Java's native `clone()` is considered flawed - many developers prefer using:
>- Copy constructors or builders
>- Factory methods
>- Libraries like Apache Common lang or ModelMapper

---
## ✳️ Runtime Memory Usage

Monitor or tune memory usage (informative only).

```java
Runtime rt = Runtime.getRuntime();
System.out.println("Max memory: " + rt.maxMemory());
System.out.println("Free memory: " + rt.freeMemory());
```

