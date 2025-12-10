# Code Samples for Title: Video 14: Managing Resources - The 'using' Statement

This file contains all code samples from the video.

## Slide 6: The 'using' Statement

### Topic 1: Manual

```csharp
// Old verbose way
StreamWriter writer = null;
try
{
    writer = new StreamWriter("data.txt");
    writer.WriteLine("Hello");
}
finally
{
    writer?.Dispose();
}
```

### Topic 2: Using Statement

```csharp
// Clean and automatic!
using (var writer = new StreamWriter("data.txt"))
{
    writer.WriteLine("Hello");
}
// Automatically disposed here!
```

## Slide 10: Cleaning up Multiple Resources

### Topic 1: Nested Using

```csharp
// Nested using statements
using (var input = new StreamReader("input.txt"))
{
    using (var output = new StreamWriter("output.txt"))
    {
        string line;
        while ((line = input.ReadLine()) != null)
        {
            output.WriteLine(line.ToUpper());
        }
    }
}
```

### Topic 2: Using Declarations

```csharp
// Much cleaner with declarations!
using var input = new StreamReader("input.txt");
using var output = new StreamWriter("output.txt");

string line;
while ((line = input.ReadLine()) != null)
{
    output.WriteLine(line.ToUpper());
}
// Both disposed here
```

