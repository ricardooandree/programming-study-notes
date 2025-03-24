<font color="#7f7f7f"><em>Sunday, 08-09-2024 - 21:07</em></font>
#programming #oop 

> [!tldr] Object oriented programming core concepts aim to create modular, reusable, and maintainable code by modelling real-world entities and interactions in a structured, flexible way

### Encapsulation
The concept of bundling data (attributes) and methods that operate on the data into a single unit, typically called an object. It restricts direct access to some of an object's components.

##### Purpose:
- Protects the internal state of an object by restricting access to its data and allowing controlled interaction through methods

##### Example:
```java
public class Person {
	private String name;

	public String getName(){
		return name;
	}

	public void setName(String name){
		this.name = name;
	}
}
```

---
### Abstraction
Hiding the complex implementation details of a class and exposing only the essential and needed features. It allows you to focus on what an object does rather than how it does it, this might be achieved through abstract classes or interfaces that act like contracts.

##### Purpose:
- Simplifies complex systems by hiding implementation details and exposing only the essential features and functionalities

##### Abstract Class:
Provide a common base with shared code and state for a single related class.

- It can't be instantiated on its own, meant to be extended (blueprint)
- Allows abstract and concrete methods (non-implemented/implemented) and attributes
```java
public abstract class Animal {
	public abstract void makeSound();
}

public class Dog extends Animal {
	@Override
	public void makeSound(){
		System.out.println("Bark bark");
	}
}
```

##### Interface

>[!danger]
> ADD:
> - compile-time polymorphism (method overloading)
> - runtime polymorphism (method overriding)
> - covariant return types
> - the instanceof operator
> - upcasting and downcasting


Define a contract that classes must follow, without providing any implementation. It's purely for defining the implementation structure.

- Traditionally don't implement concrete methods or attributes
```java
public interface Packable {
	double getWeight();
}

public class Cd implements Packable {
	private double weight;

	@Override
	public double getWeight(){
		return weight;
	}
}

public class Book implements Packable { ... }

public class Box implements Packable {
	public void add(Packable item){
		this.list.add(item);
	}
	
	@Override
	public double getWeight(){
		return weight;
	}
}
```
##### Abstract Class x Interface:
Abstract classes are used when classes share common functionality that should be inherited. It is best when you need to share code among closely related classes.
Interfaces are used when you want to specify a contract that multiple, unrelated classes should adhere to. It's ideal for defining capabilities or behaviours that don't need shared code.

---
### Inheritance

>[!danger] 
> ADD: 
> - single vs multiple inheritance (why multiple inheritance is problematic)
> - Composition vs. Inheritance (when to use which)
> - Super keyword and constructor chaining
> - Overriding vs. Overloading


Allows a new class (child or subclass) to inherit attributes and methods from an existing class (parent or superclass), establishing hierarchy or a relationship between classes.

##### Purpose:
- Promotes code reuse and hierarchical classification by allowing a class to inherit attributes and methods from another class

##### Example:
```java
public class Part {
	private String identifier;
	private String manufacturer;

	public Part(String identifier, String manufacturer){
		this.identifier = identifier;
		this.manufacturer = manufacturer;
	}

	// Getters & Setters
}

public class Car extends Part {
	private String model;

	public Car(String identifier, String manufacturer, String model){
		super(identifier, manufacturer);    // Calls Part constructor
		this.model = model;
	}

	@Override
	public void displayPartInfo(){
		super.displayPartInfo();
		System.out.println("Car Model: " + model);
	}
}
```

---
### Polymorphism
Means "many forms" and allows objects to be treated as instances of their parent class while behaving in their own unique way during compile-time

##### Purpose:
- Enables objects to be treated as instances of their parent class, allowing the same method to behave differently based on the object calling it through methods overloading or overriding

##### Example:
```java
public abstract class Animal {
	public abstract void makeSound();
}

public class Dog extends Animal {
	@Override
	public void makeSound(){
		System.out.println("Woof!");
	}
}


public class Maths {
	public int add(int a, int b){
		return a + b;
	}

	public double add(double a, double b){
		return a + b;
	}

	public int add(int a, int b, int c){
		return a + b + c;
	}
}
```