# Code Samples for Title: Methods - Organizing Your Code

This file contains all code samples from the video.

## Slide 3: Your First Method

```csharp
// Method definition
static void SayHello()
{
    Console.WriteLine("Hello from a method!");
    Console.WriteLine("Methods are awesome!");
}

// Calling the method
SayHello();  // Executes the code inside
SayHello();  // Can call it multiple times!
```

## Slide 4: Method Anatomy

```csharp
// [access] [static] [return type] [name] ([parameters])
public    static    int           Add     (int a, int b)
{
    return a + b;  // Return statement
}

// Breaking it down:
// public - who can call it
// static - belongs to class, not instance (for now)
// int - what it returns
// Add - method name
// (int a, int b) - parameters
```

## Slide 5: Parameters - Input to Methods

```csharp
// Method with parameters
static void Greet(string name, int age)
{
    Console.WriteLine($"Hello {name}, you are {age} years old");
}

// Calling with arguments
Greet("Alice", 25);  // name="Alice", age=25
Greet("Bob", 30);    // name="Bob", age=30

// Parameters are like variables that get their values
// when the method is called
```

## Slide 6: Return Values - Output from Methods

### Topic 1: Void Method

```csharp
// Void - doesn't return anything
static void PrintDouble(int num)
{
    Console.WriteLine(num * 2);
    // No return statement needed
}

PrintDouble(5);  // Just executes
```

### Topic 2: Method with Return

```csharp
// Returns an int
static int CalculateDouble(int num)
{
    return num * 2;  // MUST return an int
}

int result = CalculateDouble(5);  // result = 10
Console.WriteLine(result);
```

## Slide 7: Method Overloading

```csharp
// Same name, different parameters
static int Add(int a, int b)
{
    return a + b;
}

static int Add(int a, int b, int c)
{
    return a + b + c;
}

static double Add(double a, double b)
{
    return a + b;
}

// C# picks the right one based on arguments
Add(2, 3);           // Calls first method
Add(2, 3, 4);        // Calls second method
Add(2.5, 3.7);       // Calls third method
```

## Slide 8: Optional Parameters

```csharp
// Default values make parameters optional
static void SendEmail(string to, string subject, 
                     string cc = "", bool urgent = false)
{
    Console.WriteLine($"To: {to}");
    Console.WriteLine($"Subject: {subject}");
    if (cc != "") Console.WriteLine($"CC: {cc}");
    if (urgent) Console.WriteLine("URGENT!");
}

// Can call with 2, 3, or 4 arguments
SendEmail("bob@email.com", "Hello");
SendEmail("bob@email.com", "Hello", "alice@email.com");
SendEmail("bob@email.com", "Help!", "", true);
```

## Slide 9: Named Arguments

```csharp
static void CreateUser(string name, int age, 
                      bool isAdmin = false, 
                      bool isActive = true)
{
    // Method body
}

// Without named arguments - confusing!
CreateUser("Alice", 30, true, false);

// With named arguments - crystal clear!
CreateUser(name: "Alice", age: 30, 
          isAdmin: true, isActive: false);

// Can even reorder when using names
CreateUser(age: 30, name: "Alice");
```

## Slide 10: Pass by Value vs Reference

### Topic 1: Value Type Parameter

```csharp
static void TryChange(int num)
{
    num = 999;  // Only changes local copy
}

int x = 5;
TryChange(x);
// x is still 5!
```

### Topic 2: Reference Type Parameter

```csharp
static void TryChange(Person person)
{
    person.Name = "Changed";  // Changes object!
}

Person p = new Person { Name = "Original" };
TryChange(p);
// p.Name is now "Changed"!
```

## Slide 11: Method Scope

```csharp
static void MyMethod()
{
    int localVar = 10;  // Only exists in this method
    
    if (true)
    {
        int blockVar = 20;  // Only exists in this block
    }
    // blockVar doesn't exist here!
    
    // localVar exists until method ends
}

// Neither variable exists here!
// MyMethod();  // Each call gets fresh variables
```

## Slide 12: Expression Body Methods

### Topic 1: Traditional Method

```csharp
// Traditional block body
static int Double(int x)
{
    return x * 2;
}

static bool IsEven(int x)
{
    return x % 2 == 0;
}
```

### Topic 2: Expression Body

```csharp
// Expression body - same thing, less typing!
static int Double(int x) => x * 2;

static bool IsEven(int x) => x % 2 == 0;

// Great for simple one-liners
static string Greet(string name) => $"Hello, {name}!";
```

