<font color="#7f7f7f"><em>Tuesday, 04-03-2025 - 22:19</em></font>
#programming #oop #unit-test 

>[!info] 
>This note covers the basics of writing unit tests in Java using JUnit. It also introduces mocking and shows how to structure simple, readable, and maintainable tests.

## ✳️ What is Unit Testing?

A **unit test** is a small test that checks the behaviour of a **single piece of code** (usually a **method**) in **isolation**.

> Unit tests help ensure your code behaves as expected, prevents regressions, and supports refactoring with confidence.

### What Should You Test?

>[!list]
>- ✅ Return values of **pure functions**
>- ✅ Edge cases (e.g. empty lists, null values, zero, max/min
>- ✅ Invalid inputs (throwing exceptions)
>- ✅ Side effects (state changes, method calls)
>- ❌ Not private methods directly
>- ❌ Not external services (use mocks)

### Test Lifecycle Annotations

| Annotation    | Purpose                             |
| ------------- | ----------------------------------- |
| `@Test`       | Marks a tests method                |
| `@BeforeEach` | Runs before each test               |
| `@AfterEach`  | Runs after each test                |
| `@BeforeAll`  | Runs once before all tests (static) |
| `@AfterAll`   | Runs once after all tests (static)  |
| `@Disabled`   | Skips the test                      |

---
## ✳️ Unit Testing with JUnit 5

##### Step 1: Add the JUnit dependency w/ Maven
```xml
<dependency>
	<groupId>org.junit.jupiter</groupId>
	<artifactId>junit-jupiter</artifactId>
	<version>5.10.0</version>
	<scope>test</scope>
</dependency>
```

##### Step 2: Write the tests (basic)
```java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

public class CalculatorTest {

	@Test
	public void testAddition() {
		Calculator calc = new Calculator();
		int result = calc.add(2, 3);
		assertEquals(5, result);
	}

	@Test
	public void testDivisionByZero() {
		Calculator calc = new Calculator();
		assertThrows(ArithmeticException.class, () -> calc.divide(10, 0));
	}
}
```

> Use `@Test` to mark test methods, and assertions to check expected values.

##### Step 3: Write the tests (advanced)
```java
// Class to be tested
public class BankAccount {
	private int balance = 0;

	public void deposit(int amount) {
		this.balance += amount;
	}

	public void withdraw(int amount) {
		if (amount > balance) throw new IllegalArgumentException("Insufficient funds");
		this.balance -= amount;
	}

	public int getBalance() {
		return balance;
	}
}

// Unit tests
import org.junit.jupiter.api.*;
import static org.junit.jupiter.api.Assertions.*;

class BankAccountTest {
	private BankAccount account;

	@BeforeEach
	void setup() {
		account = new BankAccount();
		account.deposit(100);
	}

	@Test
	void testWithdraw() {
		account.withdraw(40);
		assertEquals(60, account.getBalance());
	}

	@Test
	void testWithdrawTooMuch() {
		assertThrows(IllegalArgumentException.class, () -> account.withdraw(150));
	}
}
```

---
## ✳️ What is Mocking?

**Mocking** replaces real dependencies (e.g. database, HTTP service) with fake ones, so you can test your code in isolation. So if you're testing a method that uses database information or an HTTP response you can mock that data and run the test.

### Example with Mockito

##### Step 1: Add the Mockito dependency w/ Maven
```xml
<dependency>
	<groupId>org.mockito</groupId>
	<artifactId>mockito-core</artifactId>
	<version>5.7.0</version>
	<scope>test</scope>
</dependency>
```

##### Step 2: Mock
```java
// Class to mock (in this case a service class)
public interface EmailService {
	void sendWelcomeEmail(String userEmail);
}

public class UserService {
	private EmailService emailService;

	public UserService(EmailService emailService) {
		this.emailService = emailService;
	}

	public void registerUser(String email) {
		// logic ...
		emailService.sendWelcomeEmail(email);
	}
}

// Mocking
import org.junit.jupiter.api.Test;
import static org.mockito.Mockito.*;

class UserServiceTest {

	@Test
	void testEmailIsSentOnRegistration() {
		EmailService mockEmail = mock(EmailService.class);
		UserService service = new UserService(mockEmail);

		service.registerUser("test@example.com");

		verify(mockEmail).sendWelcomeEmail("test@example.com");
	}
}
```

>[!important]
>`verify()` checks that the method was called correctly.
>You can also stub return values with `when(...).thenReturn(...)`.

### Mocking Environment Variables

Instead of accessing `System.getenv()` directly everywhere, create a **wrapper interface** that you can mock in tests.

##### Step 1: Create an Interface
```java
public interface EnvProvider {
	String get(String key);
}
```

##### Step 2: Implement the Real Provider
```java
public class SystemEnvProvider implements EnvProvider {
	@Override
	public String get(String key) {
		return System.getenv(key);
	}
}
```

##### Step 3: Use It in Your Class
```java
public class ConfigService {
	private final EnvProvider env;

	public ConfigService(EnvProvider env) {
		this.env = env;
	}

	public String getApiKey() {
		return env.get("API_KEY");
	}
}
```

##### Step 4: Mock It in Tests
```java
import static org.mockito.Mockito.*;

@Test
void testReadsApiKeyFromEnvironment() {
	EnvProvider mockEnv = mock(EnvProvider.class);
	when(mockEnv.get("API_KEY")).thenReturn("mock-key");

	ConfigService config = new ConfigService(mockEnv);

	assertEquals("mock-key", config.getApiKey());
}
```

>  This pattern gives you testability, flexibility, and keeps your cod decoupled from system-level dependencies.

