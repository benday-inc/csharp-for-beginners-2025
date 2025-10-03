# Code Samples for Title: Making Decisions with If/Else

This file contains all code samples from the video.

## Slide 3: The Basic If Statement

```csharp
int age = 25;

if (age >= 18)
{
    Console.WriteLine("You can vote!");
}

// The code inside {} only runs if condition is true
// If age was 16, nothing would print
```

## Slide 5: If/Else - Two Paths

```csharp
int temperature = 75;

if (temperature > 80)
{
    Console.WriteLine("It's hot! Wear shorts.");
}
else
{
    Console.WriteLine("It's nice. Wear whatever.");
}

// Exactly one block will execute
```

## Slide 6: Multiple Conditions with Else If

```csharp
int score = 85;

if (score >= 90)
{
    Console.WriteLine("A - Excellent!");
}
else if (score >= 80)
{
    Console.WriteLine("B - Good job!");
}
else if (score >= 70)
{
    Console.WriteLine("C - Passing");
}
else
{
    Console.WriteLine("F - Need to study more");
}
```

## Slide 7: Combining Conditions

### Topic 1: Verbose Way

```csharp
// Nested and hard to read
if (age >= 13)
{
    if (age <= 19)
    {
        Console.WriteLine("Teenager");
    }
}

if (hasPermission == true)
{
    if (isAdmin == true)
    {
        // Do something
    }
}
```

### Topic 2: Combined Conditions

```csharp
// Clean with && (AND)
if (age >= 13 && age <= 19)
{
    Console.WriteLine("Teenager");
}

// Even cleaner - booleans don't need == true
if (hasPermission && isAdmin)
{
    // Do something
}

// Use || for OR
if (day == "Saturday" || day == "Sunday")
{
    Console.WriteLine("Weekend!");
}
```

## Slide 8: The Ternary Operator

```csharp
// Traditional if/else
string message;
if (score >= 60)
{
    message = "Pass";
}
else
{
    message = "Fail";
}

// Ternary operator - same result in one line!
string message = score >= 60 ? "Pass" : "Fail";

// Format: condition ? valueIfTrue : valueIfFalse
int fee = isMember ? 0 : 25;
string status = age >= 18 ? "Adult" : "Minor";
```

## Slide 9: Switch Statements

```csharp
string day = "Monday";

switch (day)
{
    case "Monday":
    case "Tuesday":
    case "Wednesday":
        Console.WriteLine("Work day - early meeting");
        break;
    case "Thursday":
    case "Friday":
        Console.WriteLine("Work day - flexible schedule");
        break;
    default:
        Console.WriteLine("Weekend!");
        break;
}
```

## Slide 11: Null Checking

```csharp
string name = GetName();  // Might return null

// Old way - explicit null check
if (name != null)
{
    Console.WriteLine(name.Length);
}

// Modern way - null-conditional operator
Console.WriteLine(name?.Length);  // Returns null if name is null

// Null-coalescing operator
string displayName = name ?? "Guest";  // Use "Guest" if name is null
```

## Slide 12: Pattern Matching (Modern C#)

```csharp
object data = GetData();

// Type pattern matching
if (data is string text)
{
    Console.WriteLine($"String with length: {text.Length}");
}
else if (data is int number)
{
    Console.WriteLine($"Number times 2: {number * 2}");
}

// Switch expressions (C# 8+)
string result = age switch
{
    < 13 => "Child",
    < 20 => "Teenager",
    < 60 => "Adult",
    _ => "Senior"
};
```

