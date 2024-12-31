# Object Oriented Programming

## Encapsulation

#### Definition
Encapsulation is OOP concept, which bring controlled access over data by binding the data and methods into a single unit(class).

#### Problem
In procedural programming, data is often directly accessed and modified by multiple functions, making it difficult to control and protect, leading to potential inconsistencies and integrity issues.

#### Solution
Introducing classes to limit scope of data, with methods governing rules to interacting with it and access speicifer to protect it from external access.

#### Example
```csharp
using System;

public class BankAccount
{
    // Private field (data is encapsulated)
    private decimal balance;

    // Constructor to initialize the account with an initial balance
    public BankAccount(decimal initialBalance)
    {
        balance = initialBalance;
    }

    // Public method to deposit money
    public void Deposit(decimal amount)
    {
        if (amount > 0)
        {
            balance += amount;
            Console.WriteLine($"Deposited: {amount}, New balance: {balance}");
        }
        else
        {
            Console.WriteLine("Deposit amount must be positive.");
        }
    }

    // Public method to withdraw money
    public void Withdraw(decimal amount)
    {
        if (amount > 0 && amount <= balance)
        {
            balance -= amount;
            Console.WriteLine($"Withdrew: {amount}, New balance: {balance}");
        }
        else
        {
            Console.WriteLine("Insufficient funds or invalid withdrawal amount.");
        }
    }

    // Public method to get the current balance
    public decimal GetBalance()
    {
        return balance;
    }
}

class Program
{
    static void Main()
    {
        // Create an instance of BankAccount with an initial balance of 1000
        BankAccount account = new BankAccount(1000);

        // Deposit money
        account.Deposit(500);

        // Withdraw money
        account.Withdraw(200);

        // Check the balance
        Console.WriteLine("Current balance: " + account.GetBalance());
    }
}
```

## Inheritance

#### Definition
Inheritance is an OOP concept, where one class can inherit properties and methods of another class.

#### Problem
Without inheritance, the same functionality often has to be written multiple times for different types of data, leading to code duplication and making it harder to maintain or extend.

#### Solution
Inheritance enables class to reuse property and functionality of another class reducing redudancy. Also allows to represent data in a hierarchical structure.

#### Example
```csharp
using System;

// Parent class (Animal)
public class Animal
{
    public string Name { get; set; }

    // Constructor
    public Animal(string name)
    {
        Name = name;
    }

    // Method in the parent class
    public void Eat()
    {
        Console.WriteLine($"{Name} is eating.");
    }
}

// Child class (Dog) that inherits from Animal
public class Dog : Animal
{
    public string Breed { get; set; }

    // Constructor to initialize the base class and Dog-specific properties
    public Dog(string name, string breed)
        : base(name)  // Calling the base class constructor
    {
        Breed = breed;
    }

    // Method in the child class
    public void Bark()
    {
        Console.WriteLine($"{Name} barks.");
    }
}

class Program
{
    static void Main()
    {
        // Creating an object of the Dog class (which inherits from Animal)
        Dog myDog = new Dog("Buddy", "Golden Retriever");

        // Calling the method of the parent class (Animal)
        myDog.Eat();  // Output: Buddy is eating.

        // Calling the method of the child class (Dog)
        myDog.Bark(); // Output: Buddy barks.
    }
}
```

## Abstraction

#### Definition
Abstraction is an OOP concept that hides implementation details and exposes only the essential functionality, focusing on what an object does rather than how it does it.

#### Problem
Without abstraction, users are overwhelmed with unnecessary details, making the system harder to understand, maintain, and extend.

#### Solution
Introducing functions, interfaces and abstract classes to hide the implementation details, making code readable, reusable and extendable.

#### Example
```csharp
using System;

public interface IMediaPlayer
{
    void Play(); // Abstract method for playing media
}

public class AudioPlayer : IMediaPlayer
{
    public void Play()
    {
        Console.WriteLine("Playing audio file...");
    }
}

class Program
{
    static void Main()
    {
        IMediaPlayer audioPlayer = new AudioPlayer();

        audioPlayer.Play();  // Output: Playing audio file...
    }
}

```

## Polymorphism

#### Definition
Polymorphism is an OOP concept that allows objects of different types to be treated as objects of a common base type.

#### Problem
Handling different objects with similar functionality often requires explicit type checks. Also requires writing separate functions for each object type leads to lack of flexibility.

#### Solution
Polymorphism enables objects of different types to be handled uniformly using the same method, allowing code to be more extendible and flexible.

#### Example
```csharp
using System;

namespace PolymorphismWithAbstractClassExample
{
    // Abstract base class representing a document
    public abstract class Document
    {
        // Abstract method to be implemented by derived classes
        public abstract void Print();  // Every derived class must provide an implementation for this method
    }

    // Derived class representing a PDF document
    public class PdfDocument : Document
    {
        // Override the Print method for PDF documents
        public override void Print()
        {
            Console.WriteLine("Printing a PDF document...");
        }
    }

    // Derived class representing a Word document
    public class WordDocument : Document
    {
        // Override the Print method for Word documents
        public override void Print()
        {
            Console.WriteLine("Printing a Word document...");
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            // Create objects of Document type, but using specific derived types
            Document pdfDoc = new PdfDocument();  // PdfDocument treated as Document
            Document wordDoc = new WordDocument();  // WordDocument treated as Document

            // Call the Print method on different objects
            pdfDoc.Print();   // Output: Printing a PDF document...
            wordDoc.Print();  // Output: Printing a Word document...
        }
    }
}
```
