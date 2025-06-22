**SOLID Principles in TypeScript**
=====================================

The SOLID principles are five fundamental design patterns used in object-oriented programming to promote clean,
maintainable, and scalable code. Here's an overview of each principle with examples using TypeScript.

### S - Single Responsibility Principle (SRP)

*   **Definition:** A class should have only one reason to change.
*   **Example:**

    ```typescript
    // Bad practice
    class Employee {
        private name: string;
        private age: number;

        constructor(name: string, age: number) {
            this.name = name;
            this.age = age;
        }

        getAge() {
            return this.age;
        }

        updateName(newName: string) {
            this.name = newName;
        }
    }

    // Good practice
    class Employee {
        private name: string;

        constructor(name: string) {
            this.name = name;
        }

        getName() {
            return this.name;
        }

        setName(newName: string) {
            this.name = newName;
        }
    }
    ```

In the example above, the `Employee` class now has only one responsibility: managing employee information.

### O - Open-Closed Principle (OCP)

*   **Definition:** A class should be open for extension but closed for modification.
*   **Example:**

    ```typescript
    // Bad practice
    class PaymentMethod {
        processPayment(amount: number) {
            // Concrete implementation
            return amount * 0.9;
        }
    }

    // Good practice
    abstract class PaymentMethod {
        abstract processPayment(amount: number): number;

        defaultFee(): number {
            return 0.1; // default fee for all payment methods
        }

        processAmount(amount: number) {
            const payment = this.processPayment(amount);
            return payment - this.defaultFee();
        }
    }

    class CreditCard extends PaymentMethod {
        processPayment(amount: number): number {
            // Concrete implementation for credit card
            return amount * 0.9;
        }
    }
    ```

In the example above, we've introduced an abstract `PaymentMethod` class that is open for extension (new payment
methods can be added) but closed for modification (the existing method cannot be changed).

### L - Liskov Substitution Principle (LSP)

*   **Definition:** Derived classes should be substitutable for their base classes.
*   **Example:**

    ```typescript
    // Bad practice
    class Bird {
        fly() {
            console.log("Flying");
        }
    }

    class Penguin extends Bird {
        // No way to implement flying, will raise an error
    }
    ```

    In the example above, `Penguin` cannot be used as a substitute for `Bird` because it does not have a way to "fly".

    ### I - Interface Segregation Principle (ISP)

    *   **Definition:** A client should not be forced to depend on interfaces it doesn't use.
    *   **Example:**

    ```typescript
    // Bad practice
    interface PaymentMethod {
        processPayment(amount: number);
    }

    class CreditCard implements PaymentMethod {
        processPayment(amount: number) {
            // Concrete implementation for credit card
        }
    }

    class PayPal implements PaymentMethod {
        processPayment(amount: number) {
            // Concrete implementation for PayPal
        }
    }
    ```

In the example above, both `CreditCard` and `PayPal` are forced to implement the `processPayment` method even
though they don't need it.

### D - Dependency Inversion Principle (DIP)

*   **Definition:** High-level modules should not depend on low-level modules. Both should depend on abstractions.
*   **Example:**

    ```typescript
    // Bad practice
    class Logger {
        log(message: string) {
            console.log(message);
        }
    }

    class Service {
        private logger: Logger;

        constructor(logger: Logger) {
            this.logger = logger;
        }

        doSomething() {
            this.logger.log("Doing something");
        }
    }
    ```

In the example above, `Service` is tightly coupled to `Logger`.