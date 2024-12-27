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

