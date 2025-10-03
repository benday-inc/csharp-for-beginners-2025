# Code Samples for Title: Your First C# Program

This file contains all code samples from the video.

## Slide 3: Creating Our Project

```bash
# Navigate to your dev folder
cd C:\Dev

# Create a new console application
dotnet new console -n HelloWorld

# Move into the project folder
cd HelloWorld
```

## Slide 5: Your First C# Code

```csharp
// See https://aka.ms/new-console-template for more information
Console.WriteLine("Hello, World!");
```

## Slide 6: Running Your Program

```bash
# Make sure you're in the HelloWorld folder
dotnet run

# Output:
Hello, World!
```

## Slide 8: Let's Make It Interactive

### Topic 1: Static Message

```csharp
// Original
Console.WriteLine("Hello, World!");
```

### Topic 2: Interactive Program

```csharp
// Interactive version
Console.WriteLine("What's your name?");
string name = Console.ReadLine();
Console.WriteLine($"Hello, {name}!");
```

## Slide 11: Let's Add Some Logic

```csharp
Console.WriteLine("What's your name?");
string name = Console.ReadLine();

if (name.Length > 10)
{
    Console.WriteLine($"Wow, {name} is a long name!");
}
else
{
    Console.WriteLine($"Hello, {name}!");
}
```

