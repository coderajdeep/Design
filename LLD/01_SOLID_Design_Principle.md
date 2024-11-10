_The **SOLID** design principles are a set of five guidelines/rules that help software developers create more maintainable, scalable, and robust object-oriented software._ Introduced by Robert C. Martin, these principles aim to improve code quality and reduce dependencies. The acronym SOLID stands for:

### 1. **Single Responsibility Principle (SRP)**

**Definition**: A class should have only one reason to change, meaning it should only have one job or responsibility.

**Example**: Suppose we have a class `Invoice` that handles creating, printing, and saving invoices to the database. This would violate SRP because `Invoice` has multiple responsibilities.

```java
class Invoice {
    public void createInvoice() { /* logic to create invoice */ }
    public void printInvoice() { /* logic to print invoice */ }
    public void saveToDatabase() { /* logic to save invoice to database */ }
}
```

**Solution**: Split the responsibilities into separate classes:

```java
class Invoice { /* logic to create an invoice */ }

class InvoicePrinter {
    public void printInvoice(Invoice invoice) { /* logic to print invoice */ }
}

class InvoiceRepository {
    public void saveToDatabase(Invoice invoice) { /* logic to save to database */ }
}
```

Now, each class has a single responsibility: `Invoice` handles invoice creation, `InvoicePrinter` handles printing, and `InvoiceRepository` handles saving.

---

### 2. **Open-Closed Principle (OCP)**

**Definition**: (Software entities) Class should be open for extension but closed for modification.

**Example**: Let's say we have a `DiscountCalculator` class that calculates discounts for different customer types.

```java
class DiscountCalculator {
    public double calculateDiscount(String customerType, double amount) {
        if (customerType.equals("Regular")) {
            return amount * 0.1;
        } else if (customerType.equals("VIP")) {
            return amount * 0.2;
        }
        return 0;
    }
}
```

**Problem**: Every time we add a new customer type, we must modify `DiscountCalculator`.

**Solution**: Use inheritance or interfaces to allow for extensions:

```java
// Better example
// Instead of creating concreat class, we can use DiscountCalculator interface
@FunctionalInterface
interface DiscountCalculator {
    public double calculateDiscount(double amount);
}

public class MainTest {
    public static void main(String[] args) {
        DiscountCalculator regularDiscount = (double amount) -> amount * 0.1;
        DiscountCalculator vipDiscount = (double amount) -> amount * 0.2;
        DiscountCalculator noDiscount = (double amount) -> 0;
        System.out.println(regularDiscount.calculateDiscount(1000));
        System.out.println(vipDiscount.calculateDiscount(1000));
        System.out.println(noDiscount.calculateDiscount(1000));
    }
}

// Same thing in different way
interface Discount {
    double apply(double amount);
}

class RegularDiscount implements Discount {
    public double apply(double amount) { return amount * 0.1; }
}

class VIPDiscount implements Discount {
    public double apply(double amount) { return amount * 0.2; }
}

class DiscountCalculator {
    public double calculateDiscount(Discount discount, double amount) {
        return discount.apply(amount);
    }
}
public class MainTest {
    public static void main(String[] args) {
        DiscountCalculator discountAmount = new DiscountCalculator();
        RegularDiscount regularDiscount = new RegularDiscount();
        double totalAmount = 1000;
        double totalDiscount = discountAmount.calculateDiscount(regularDiscount, totalAmount);
        System.out.println(totalDiscount);
    }
}
```

Now, we can add new discounts without changing `DiscountCalculator`.

---

### 3. **Liskov Substitution Principle (LSP)**

**Definition**: Objects of a superclass should be replaceable with objects of subclasses without affecting the correctness of the program.

**Example**: Suppose we have a superclass `Bird` and a subclass `Penguin`. `Bird` has a method `fly()`.

```java
class Bird {
    public void fly() { /* logic to fly */ }
}

class Penguin extends Bird {
    @Override
    public void fly() {
        throw new UnsupportedOperationException("Penguins can't fly");
    }
}
```

**Problem**: Penguins cannot fly, so `fly()` doesn’t make sense for this subclass.

**Solution**: Instead of using inheritance here, we could separate birds into `FlyingBird` and `NonFlyingBird` classes:

```java
abstract class Bird { /* Common properties and methods */ }

class FlyingBird extends Bird {
    public void fly() { /* logic to fly */ }
}

class Penguin extends Bird { /* No fly method */ }
```

This way, we avoid forcing non-flying birds to implement behavior they don’t support.

---

### 4. **Interface Segregation Principle (ISP)**

**Definition**: Clients should not be forced to depend on interfaces they do not use.

This means that when we design interfaces, each one should be specific and focused on a particular aspect of functionality, rather than a large, "fat" interface that includes methods irrelevant to some classes.

### A Practical Example of ISP

Imagine we have an application that models different types of employees. We create an interface `Employee` to represent their responsibilities:

```java
interface Employee {
    void work();
    void attendMeeting();
    void submitReport();
}
```

Now, let’s say we implement this interface for two different types of employees: `Manager` and `Developer`. Both can use these methods.

```java
class Manager implements Employee {
    public void work() { /* work logic */ }
    public void attendMeeting() { /* meeting logic */ }
    public void submitReport() { /* report logic */ }
}

class Developer implements Employee {
    public void work() { /* work logic */ }
    public void attendMeeting() { /* meeting logic */ }
    public void submitReport() { /* report logic */ }
}
```

So far, so good! But now, let’s add another type of employee, say a `RobotWorker`. This is an automated system that performs certain tasks but doesn’t attend meetings or submit reports.

```java
class RobotWorker implements Employee {
    public void work() { /* robot-specific work */ }
    public void attendMeeting() { 
        // Robot doesn’t attend meetings, so this is irrelevant.
        throw new UnsupportedOperationException("Robot doesn't attend meetings");
    }
    public void submitReport() { 
        // Robot doesn’t submit reports, so this is also irrelevant.
        throw new UnsupportedOperationException("Robot doesn't submit reports");
    }
}
```

Now, `RobotWorker` is forced to implement methods it doesn’t need. This violates ISP because `RobotWorker` depends on parts of the `Employee` interface that are irrelevant to it.

### Solution: Split the Interface

The solution is to break down the `Employee` interface into more specific, focused interfaces. For example:

```java
interface Worker {
    void work();
}

interface Attendee {
    void attendMeeting();
}

interface Reporter {
    void submitReport();
}
```

Now, we can have each type of employee implement only the interfaces that are relevant to it:

```java
class Manager implements Worker, Attendee, Reporter {
    public void work() { /* work logic */ }
    public void attendMeeting() { /* meeting logic */ }
    public void submitReport() { /* report logic */ }
}

class Developer implements Worker, Attendee, Reporter {
    public void work() { /* work logic */ }
    public void attendMeeting() { /* meeting logic */ }
    public void submitReport() { /* report logic */ }
}

class RobotWorker implements Worker {
    public void work() { /* robot-specific work */ }
}
```

Now, `RobotWorker` only implements the `Worker` interface and isn’t forced to implement irrelevant methods. This makes each class more focused and easier to maintain.

---

### 5. **Dependency Inversion Principle (DIP)**

**Definition**: High-level modules should not depend on low-level modules; both should depend on abstractions. Also, abstractions should not depend on details; details should depend on abstractions.

**Example**: Let’s say `CustomerService` directly depends on `EmailService`.

```java
class EmailService {
    public void sendEmail(String message) { /* logic to send email */ }
}

class CustomerService {
    private EmailService emailService;

    public CustomerService() {
        this.emailService = new EmailService();
    }

    public void notifyCustomer(String message) {
        emailService.sendEmail(message);
    }
}
```

**Problem**: If we want to switch to a different notification service, we’d need to modify `CustomerService`.

**Solution**: Use an interface for `NotificationService` that `EmailService` implements.

```java
interface NotificationService {
    void send(String message);
}

class EmailService implements NotificationService {
    public void send(String message) { /* logic to send email */ }
}

class SMSService implements NotificationService {
    public void send(String message) { /* logic to send SMS */ }
}

class CustomerService {
    private NotificationService notificationService;

    public CustomerService(NotificationService notificationService) {
        this.notificationService = notificationService;
    }

    public void notifyCustomer(String message) {
        notificationService.send(message);
    }
}
```

Now, we can inject any `NotificationService` implementation into `CustomerService`, making it more flexible.

---

These principles help make code easier to extend, test, and maintain. By applying SOLID principles, software design can become more modular and adaptable to change.