<font color="#7f7f7f"><em>Sunday, 08-09-2024 - 21:07</em></font>
#programming #oop 

> [!info] 
> Object oriented programming aims to create modular, reusable, and maintainable code by modelling real-world entities and their  interactions through structured, abstract, and encapsulated designs.

## ✳️ Encapsulation

Encapsulation is the concept of bundling data (attributes) and the methods that operate on that data into a single unit -typically an object- and restricting direct access to some of that object's components.

##### Purpose:
- Protects the internal state of an object
- Promotes modularity and data integrity
- Allows controlled access through public methods (Getters/Setters)
  
##### 💡Example - Encapsulation
```java
public class Person {
	private String name;

	// Getter
	public String getName(){
		return name;
	}

	// Setter
	public void setName(String name){
		this.name = name;
	}
}
```

---
## ✳️ Abstraction

Abstraction is the practice of hiding the internal implementation details while exposing only the essential features of an object. It focuses on **what** and object does rather than **how** it does it.

##### Purpose:
- Reduces complexity by hiding internal details
- Promotes a clean and simplified interface for interaction

### 🔷 Abstract Class

An abstract class provides a partial *blueprint* that can include both **concrete (implemented)** and **abstract (declared only/non-implemented)** methods.

- Cannot be instantiated (meant to be extended - blueprint)
- Used for classes with a shared base structure or logic

##### 💡 Example - Abstract Class
```java
public abstract class Animal {
	public abstract void makeSound();              // Abstract method
	
	public int add(int a, int b){ return a + b; }  // Concrete method
} 

public class Dog extends Animal {
	@Override
	public void makeSound(){
		System.out.println("Bark bark");
	}
}
```

>[!important]
> Abstract **METHODS** can define return types, parameters or none.
> Abstract **CLASSES** can also declare attributes e.g. `protected String brand` which are inherited by the subclasses and **can** have constructors.
> Traditionally abstract classes do not implement concrete methods.

### 🔷 Interface

An Interface defines a set of method signatures (abstract) that implementing classes must fulfill (contract).

- Classes can implement multiple interfaces
- Interface methods are implicitly `public` and `abstract`
- Attributes are implicitly `public static final`

##### 💡 Example - Interface
```java
public interface Packable {
	int MAX_WEIGHT = 100;  // Acts as static final
	double getWeight();
	double sumWeight(double w1, double w2);
}

public class Cd implements Packable {
	private double weight;

	@Override
	public double getWeight(){
		return weight;
	}

	@Override
	public double sumWeight(double w1, double w2){
		return w1 + w2;
	}
}
```

>[!important]
>**Interfaces** may instantiate attributes but only as `static final` by default.
>Interfaces **CANNOT** implement methods (concrete methods)!

### 🔄 Abstract Class vs. Interface

| Feature           | Abstract Class                         | Interface                               |
| ----------------- | -------------------------------------- | --------------------------------------- |
| **Purpose**       | Share common code in a class hierarchy | Define a contract across types          |
| **Instantiation** | Cannot be instantiated                 | Cannot be instantiated                  |
| **Method Types**  | Concrete + Abstract                    | Abstract                                |
| **Constructors**  | Yes                                    | No                                      |
| **Inheritance**   | Single Inheritance                     | Multiple Inheritance                    |
| **Use Case**      | Closely related classes                | Unrelated classes with shared behaviour |

> The major difference is that abstract classes can have concrete and abstract methods while interfaces can only have abstract methods.

---
## ✳️ Inheritance

Inheritance allows a class (subclass/child) to inherit attributes and methods from another class (superclass/parent), forming a hierarchy.

##### Purpose:
- Enables code reuse
- Supports a natural class hierarchy

##### 💡 Example - Inheritance
```java
public class Book {
	private String author;
	private String title;

	public Book(String author, String title){
		this.author = author;
		this.title = title;
	}

	// Getters & Setters

	public void displayBookInfo(){
		System.out.println("Author: " + author + " Title: " + title);
	}
}

public class Library extends Book {
	private String name;

	public Library(String author, String title, String name){
		super(author, title);    // Calls Part constructor
		this.name = name;
	}

	@Override
	public void displayBookInfo(){
		super.displayBookInfo();
		System.out.println("Library Name: " + name);
	}

	public void bookAuthors(){
		System.out.println("Author: " + super.getAuthor());
	}
}
```

### 🔷 Single vs. Multiple Inheritance

Java supports **single inheritance** with classes to avoid the "diamond problem". -> Ambiguity when two parent classes define the same method.

**Multiple inheritance** is only supported via interfaces.


### 🔷 Composition vs. Inheritance

**Inheritance:** "is-a" relationship. -> `Car is-a Vehicle`

**Composition:** "has-a" relationship (preferred when behaviour needs flexibility or change over time). -> `Car has-a Engine`

##### When to use Composition?

Behaviour can be shared but doesn't require class hierarchy.
You want more flexibility (change implementation at runtime).

```java
public Class Engine {
	public void start() { System.out.println("Engine started"); }
}

public Class Car {
	private Engine engine;

	public Car(){
		this.engine = new Engine();
	}

	public void start(){
		engine.start();
		System.out.println("Car is now running");
	}
}
```

### 🔷 Super Keyword & Constructor Chaining

`super()` is used to call the constructor or methods of the parent class.
Useful for reusing initialization logic and avoiding duplication.

---
## ✳️ Polymorphism

Polymorphism means "many forms" and allows objects to be treated as instances of their parent class while invoking their overridden behaviour.

##### Purpose:
- Treat objects as instances of their parent type while maintaining specific behaviours.
- Supports flexibility and decoupling.

### 🔷 Compile-time Polymorphism (Overloading)

Multiple methods with the same name but different parameters.

```java
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

> Also works for constructors.

### 🔷 Runtime Polymorphism (Overriding)

Behaviour determined at runtime using method overriding, changing the original behaviour. The subclass overrides a method from the parent. (Redefining a method implementation).

```java
public abstract class Animal{
	public abstract void makeSound();
}

public class Cat extends Animal {
	@Override
	public void makeSound(){
		System.out.println("Meow!");
	}
}
```

### 🔷 Covariant Return Types

Allows overriding a method with a more specific return type than the one declared in the parent class.

```java
class Animal {}
class Dog extends Animal {}

class Parent {
	Animal getAnimal() { return new Animal(); }
}

class Child extends Parent {
	@Override
	Dog getAnimal() { return new Dog(); }  // Overrode the return type
}
```

### 🔷 `instanceof` Operator

Used to check an object's actual type at runtime before casting.

```java
if (animal instanceof Dof){
	((Dof) animal).bark();
}
```

### 🔷 Upcasting & Downcasting

Reference to casting from one class to another.

**Upcasting:** Subclass -> Superclass (safe)

**Downcasting:** Superclass -> Subclass (requires `instanceof` check to avoid `ClassCastException`)

```java
Animal a = new Dog(); // Upcasting (safe)
Dog d = (Dog) a;      // Downcasting (safe only if 'a' is actually a Dog)
```