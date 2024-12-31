# SOLID Design Principle

## Single Responsibility Principle(SRP)

**Definition**: A class should have only one reason to change, meaning it should have only one responsibility.

**Voilation**: The UserService class handles both user registration and email sending, which are separate responsibilities.
```csharp
public class UserService
{
    public void RegisterUser(string username)
    {
        // Register the user logic
        Console.WriteLine($"User {username} registered.");
    }

    public void SendWelcomeEmail(string email)
    {
        // Logic to send a welcome email
        Console.WriteLine($"Sending email to {email}");
    }
}
```

**Adhere**: We split the responsibilities into two different classes i.e. UserRegistrationService and EmailService.
```csharp
// Class responsible for user registration
public class UserRegistrationService
{
    public void RegisterUser(string username)
    {
        // Register user logic
        Console.WriteLine($"User {username} registered.");
    }
}

// Class responsible for sending a welcome email
public class EmailService
{
    public void SendWelcomeEmail(string email)
    {
        // Logic to send a welcome email
        Console.WriteLine($"Sending welcome email to {email}");
    }
}
```

## Open Closed Principle(OCP)

**Definition**: Classes should be open for extension but closed for modification.

**Voilation**:Adding a new payment type requires modifying the ProcessPayment method.
```csharp
public class PaymentProcessor
{
    public void ProcessPayment(string paymentType)
    {
        if (paymentType == "CreditCard") { /* Process CreditCard */ }
        else if (paymentType == "PayPal") { /* Process PayPal */ }
    }
}
```

**Adhere**: New payment types can be added by creating new classes without modifying existing ones.
```csharp
public interface IPayment
{
    void ProcessPayment();
}

public class CreditCardPayment : IPayment
{
    public void ProcessPayment() { /* Process CreditCard */ }
}

public class PayPalPayment : IPayment
{
    public void ProcessPayment() { /* Process PayPal */ }
}

public class PaymentProcessor
{
    public void ProcessPayment(IPayment payment)
    {
        payment.ProcessPayment();
    }
}
```

## Liskow Substitution Principle(LSP)

**Definition**: Objects of a superclass should be replaceable with objects of a subclass without affecting the correctness of the program.

**Voilation**: The `Bicycle` class overrides `StartEngine()` and throws an exception. Replacing a `Vehicle` with a `Bicycle` breaks LSP because `StartEngine()` is not valid for bicycles, causing errors.
```csharp
public abstract class Vehicle
{
    public abstract void StartEngine();
    public abstract void Accelerate();
}

public class Bicycle : Vehicle
{
    public override void StartEngine()
    {
        throw new NotImplementedException("Bicycle do not have engines!");
    }

    public override void Accelerate()
    {
        Console.WriteLine("Accelerate with pedal");
    }
}

public class Bike : Vehicle
{
    public override void StartEngine()
    {
        Console.WriteLine("Starting v8 engine! vroom!!!");
    }

    public override void Accelerate()
    {
        Console.WriteLine("Accelerate with gear");
    }
}
```

**Adhere**: Moving `StartEngine()` method to a seperate interface so that only motor based vehicle can use it.
```csharp
public interface IMotor
{
    void StartEngine();
}

public abstract class Vehicle
{
    public abstract void Accelerate();
}

public class Bicycle : Vehicle
{
    public override void Accelerate()
    {
        Console.WriteLine("Accelerate with pedal");
    }
}

public class Bike : Vehicle, IMotor
{
    public void StartEngine()
    {
        Console.WriteLine("Starting v8 engine! vroom!!!");
    }

    public override void Accelerate()
    {
        Console.WriteLine("Accelerate with gear");
    }
}
```

## Interface Segregation Principle(ISP)

**Definition**: No client should be forced to depend on methods it does not use.

**Voilation**: The `IMediaPlayer` interface forces all classes to implement both audio and video functionality, even if they don't support it.
```csharp
// Violates ISP because all players must implement methods for both audio and video
public interface IMediaPlayer
{
    void PlayAudio(string fileName);
    void PlayVideo(string fileName);
}

// A class that plays both video & audio by implementing methods
public class VlcPlayer : IMediaPlayer
{
    public void PlayAudio(string fileName)
    {
        Console.WriteLine($"Playing audio: {fileName}");
    }

    public void PlayVideo(string fileName)
    {
        Console.WriteLine($"Playing video: {fileName}");
    }
}

// A class that only plays audio, but is forced to implement video method
public class IpodPlayer : IMediaPlayer
{
    public void PlayAudio(string fileName)
    {
        Console.WriteLine($"Playing video: {fileName}");
    }

    public void PlayVideo(string fileName)
    {
        // Forced to implement even though it's unnecessary
        throw new NotImplementedException("VideoPlayer cannot play video.");
    }
}
```

**Adhere**: Split the IMediaPlayer interface into smaller, more specific interfaces: IAudioPlayer and IVideoPlayer.
```csharp
// Interfaces that are more specific, adhering to ISP
public interface IAudioPlayer
{
    void PlayAudio(string fileName);
}

public interface IVideoPlayer
{
    void PlayVideo(string fileName);
}

// A class that plays both audio and video, implements both interfaces
public class VlcPlayer : IAudioPlayer, IVideoPlayer
{
    public void PlayAudio(string fileName)
    {
        Console.WriteLine($"Playing audio: {fileName}");
    }

    public void PlayVideo(string fileName)
    {
        Console.WriteLine($"Playing video: {fileName}");
    }
}

// A class that only plays audio, implements IAudioPlayer
public class IpodPlayer : IAudioPlayer
{
    public void PlayAudio(string fileName)
    {
        Console.WriteLine($"Playing audio: {fileName}");
    }
}
```

## Dependency Inversion Principle(DIP)

**Definition**: High-level modules should not depend on low-level modules. Both should depend on abstractions.

**Voilation**: High-level class OrderService directly depends on a low-level class EmailService for sending notifications making it tighly coupled.
```csharp
// Low-level module (EmailService)
public class EmailService
{
    public void SendEmail(string message)
    {
        Console.WriteLine("Sending email: " + message);
    }
}

// High-level module (OrderService) directly depends on EmailService
public class OrderService
{
    private EmailService _emailService;

    public OrderService()
    {
        _emailService = new EmailService();  // Direct dependency on low-level module
    }

    public void PlaceOrder(string orderDetails)
    {
        Console.WriteLine("Order placed: " + orderDetails);
        _emailService.SendEmail("Your order has been placed: " + orderDetails); // Tight coupling
    }
}
```

**Adhere**: Create a notification interface, and inject it into `OrderService` and `EmailService` via constructor or method parameters.
```csharp
// Abstraction for sending notifications
public interface INotificationService
{
    void SendNotification(string message);
}

// Low-level module (EmailService) now implements INotificationService
public class EmailService : INotificationService
{
    public void SendNotification(string message)
    {
        Console.WriteLine("Sending email: " + message);
    }
}

// High-level module (OrderService) depends on the abstraction, not on the concrete implementation
public class OrderService
{
    private readonly INotificationService _notificationService;

    // Dependency Injection (constructor injection)
    public OrderService(INotificationService notificationService)
    {
        _notificationService = notificationService;
    }

    public void PlaceOrder(string orderDetails)
    {
        Console.WriteLine("Order placed: " + orderDetails);
        _notificationService.SendNotification("Your order has been placed: " + orderDetails);
    }
}
```