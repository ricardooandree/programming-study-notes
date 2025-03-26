<font color="#7f7f7f"><em>Wednesday, 11-09-2024 - 15:31</em></font>
#programming #oop #principles 

>[!info] 
>The SOLID principles are five core guidelines for writing clean, maintainable, and scalable object-oriented code. Each principle helps avoid common design pitfalls like tight coupling, low cohesion, and code that’s hard to test or extend.

## ✳️ Overview: What is SOLID?

SOLID is an acronym that stands for:
- **S** - Single Responsibility Principle
- **O** - Open/Closed Principle
- **L** - Liskov Substitution Principle
- **I** - Interface Segregation Principle
- **D** - Dependency Inversion Principle

These principles help guide class design and architecture in OOP.

| Principle | Summary                                     | Focus                |
| --------- | ------------------------------------------- | -------------------- |
| SRP       | One reason to change                        | Class responsibility |
| OCP       | Open for extension, closed for modification | Extensibility        |
| LSP       | Subtypes must be substitutable              | Correct inheritance  |
| ISP       | Split large interfaces                      | Interface design     |
| DIP       | Depend on abstractions                      | Loose coupling       |

---
## ✳️ S - Single Responsibility Principle (SRP)

> **A class should have only one reason to change.**

Each class or module should have very closely related tasks. 

One job, one class.
Just like a coffee machine shouldn't also make toasts, each class should have one and only one job or reason to change.

❌ **Example (Violation)**
```java
public class Invoice {
	public void calculateTotal() { ... }
	public void printInvoice() { ... }    // Printing = another responsibility
	public void saveToFile() { ... }      // Persistence = another responsibility
}
```

✔️ **Applying SRP**
```java
public class Invoice {
	public void calculateTotal() { ... }
}

public class InvoicePrinter {
	public void print(Invoice invoice) { ... }
}

public class InvoiceRepository {
	public void save(Invoice invoice) { ... }
}
```

>Each class now has a single concern and can evolve independently.

**Extra Example**
A Student class shouldn't have methods like:
- `save()` - saves student into the database
- `email()` - emails the student
- `enrol()` - enrol the student in a course

These should be separated instances like:
- Student Class
- EmailService Class
- EnrollementService Class
- StudentRepository Class

---
## ✳️ O - Open/Closed Principle (OCP)

> **Software entities (classes, modules, functions) should be open for extension but closed for modification.**

You should be able to add new behaviour without changing existing code (open to **extension** but closed to **modification**).

Add new doors, don't break the wall
Your code should be open to adding new features but closed to editing existing, working code - like plugging in a new appliance without rewiring the house.

✔️ **Applying OCP - Using Inheritance or Interfaces**
```java
public abstract class Shape {
	public abstract double area();
}

public class Circle extends Shape { ... }
public class Rectangle extends Shape { ... }

public class AreaCalculator {
	public double totalArea(List<Shape> shapes) {
		double total = 0;
		for (Shape shape : shapes) {
			total += shape.area();  // extensible without changing logic
		}
		return total;
	}
}

```

> Add new `Shape` types without modifying `AreaCalculator`.
> Decorator Pattern helps to implement OCP.

---
## ✳️ L - Liskov Substitution Principle (LSP)

> **Subtypes must be substitutable for their base types without breaking program correctness.**
> **A Child class should be able to do everything a Parent class can.**

In other words: If `S` is a subtype of `T`, then objects of type `T` should be replaceable with objects of type `S` without altering the correctness of the program.

If it walks like a duck...
Subclasses should be usable in place of their parent class without causing bugs - like swapping a regular USB mouse for a wireless one and expecting it to work the same.

❌ **Example (Violation)**
```java
public class Bird {
	public void fly() { ... }
}

public class Ostrich extends Birds {
	@Override
	public void fly() {
		throw new UnsupportedOperationException("Ostriches can't fly");
	}
}
```

Ostrich *is-a* Bird, but **can't be substituted** in places where `Bird` is expected.

✔️ **Applying LSP**
```java
public interface Flyable {
	void fly();
}

public class Sparrow implements Flyable { ... }
```

> Favour composition or segregated interfaces to avoid LSP violations.
---
## ✳️ I - Interface Segregation Principle (ISP)

>**No client should be forced to depend on methods it does not use.**

Split large, "fat" interfaces into smaller, more specific ones.

Don't make me implement buttons I don't use.
Clients shouldn't be forced to depend on features they don't need - like a basic remote control that only needs volume and power, not Netflix and Bluetooth.

❌ **Example (Violation)**
```java
public interface Machine {
	void print();
	void scan();
	void fax();
}

public class OldPrinter implements Machine {
	public void print() { ... }
	public void scan() {
		throw new UnsupportedOperationException();
	}
	public void fax() {
		throw new UnsupportedOperationException();
	}
}
```

✔️ **Applying ISP**
```java
public interface Printer {
	void print();
}

public interface Scanner {
	void scan();
}
```

> Clients only implement what they actually need.
---
## ✳️ D - Dependency Inversion Principle (DIP)

> **High-level modules should not depend on low-level modules.**
> **Both should depend on abstractions.**

And:

> **Abstractions should not depend on details.**
> **Details should depend on abstractions.**

Rely on contracts, not concrete tools.
High-level code shouldn't depend on specific low-level details - it should depend on interfaces, like calling a delivery service instead of choosing a specific driver.

❌ **Example (Violation)**
```java
public class LightSwitch {
	private LightBulb bulb = new LightBulb();  // depends on concrete class

	public void toggle() {
		bulb.turnOn();
	}
}
```

✔️ **Applying DIP**
```java
public interface Switchable {
	void turnOn();
}

public class LightBulb implements Switchable {
	public void turnOn() { ... }
}

public class LightSwitch {
	private Switchable device;

	public LightSwitch(Switchable device) {
		this.device = device;
	}

	public void toggle() {
		device.turnOn();
	}
}
```

> This allows easy substitution, testing (mocks), and less coupling.
