# Records
In **Java**, a record is a special, unrestricted class introduced in Java 14, designed to server as a pure **data carrier**. Getting rid of the useless stuff (boilerplate code) for classes created only for immutable classces.

## Functionality
When a **record** is declared, Java automatically generates:
- **Private final fields** for each parameter in the header.
- **Public getter methods** named after the component (e.g. `name()` instead of `getName()` ).
- A **canonical constructor** initializing all fields.
- `equals()` method implemented to compare records by field not by object identity.
- A human-readable `toString()` method listing all components.

## Basic Syntax
Instead of writing a 30-line traditional JavaBean with fields, getters, `equals()`, and `toString()`:
```java
public record Person(String name, int age) {}
```

### Usage Example:
```java
Person alice = new Person("Alice", 30);

// Component accessors (no "get" prefix)
System.out.println(alice.name()); // Outputs: Alice
System.out.println(alice.age());  // Outputs: 30

// Auto-generated toString
System.out.println(alice); // Outputs: Person[name=Alice, age=30]

// Value-based equality
Person alice2 = new Person("Alice", 30);
System.out.println(alice.equals(alice2));
```

