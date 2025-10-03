# Code Samples for Title: Working with Strings in C#

This file contains all code samples from the video.

## Slide 3: String Basics

```csharp
string name = "Ben";
string greeting = "Hello, " + name;  // Concatenation
string modern = $"Hello, {name}";    // Interpolation

// Strings are objects with methods
string upper = name.ToUpper();       // "BEN"
int length = name.Length;            // 3
bool starts = name.StartsWith("B");  // true
```

## Slide 4: String Immutability

```csharp
string original = "Hello";
string modified = original.ToUpper();

// original is STILL "Hello"
// modified is "HELLO" (new string object!)

// This doesn't change the string:
original.Replace("H", "J");  // Returns new string!

// You must capture the result:
original = original.Replace("H", "J");  // Now it's "Jello"
```

## Slide 6: String Comparison

### Topic 1: Wrong Way

```csharp
// Case-sensitive by default!
if (userInput == "yes")  // Won't match "Yes"
{
    // Might not execute
}

// This is also inefficient
if (userInput.ToLower() == "yes")
{
    // Creates new string just for comparison
}
```

### Topic 2: Right Way

```csharp
// Use StringComparison for efficiency
if (userInput.Equals("yes", 
    StringComparison.OrdinalIgnoreCase))
{
    // Matches "yes", "Yes", "YES", etc.
}

// Or for current culture rules
if (string.Equals(userInput, "yes", 
    StringComparison.CurrentCultureIgnoreCase))
{
    // Culture-aware comparison
}
```

## Slide 7: The Concatenation Problem

```csharp
// DON'T DO THIS IN A LOOP!
string result = "";
for (int i = 0; i < 1000; i++)
{
    result += i.ToString() + ", ";  // Creates 2000 new strings!
}

// This creates approximately 2000 string objects
// Each concatenation creates a new string
// Old strings become garbage
```

## Slide 8: StringBuilder to the Rescue

### Topic 1: String Concatenation

```csharp
// Slow: O(n²) complexity
string result = "";
for (int i = 0; i < 10000; i++)
{
    result += i + ", ";
}
// Takes ~500ms, creates 20,000 strings
```

### Topic 2: StringBuilder

```csharp
// Fast: O(n) complexity  
var sb = new StringBuilder();
for (int i = 0; i < 10000; i++)
{
    sb.Append(i).Append(", ");
}
string result = sb.ToString();
// Takes ~2ms, creates 1 string!
```

## Slide 10: String Interpolation Magic

```csharp
// Basic interpolation
string name = "Ben";
int age = 42;
string bio = $"Name: {name}, Age: {age}";

// Expressions work too!
string msg = $"Next year you'll be {age + 1}";

// Formatting inline
decimal price = 19.99m;
string display = $"Total: {price:C}";  // "Total: $19.99"

// Conditional logic
string status = $"Status: {(age >= 18 ? "Adult" : "Minor")}";
```

## Slide 11: StringReader and StringWriter

```csharp
// StringReader - read string like a file
string data = "Line1\nLine2\nLine3";
using (var reader = new StringReader(data))
{
    string line;
    while ((line = reader.ReadLine()) != null)
    {
        Console.WriteLine($"Read: {line}");
    }
}

// StringWriter - write to string buffer
using (var writer = new StringWriter())
{
    writer.WriteLine("Header");
    writer.WriteLine("Data");
    string result = writer.ToString();
}
```

## Slide 12: Escape Sequences and Verbatim Strings

### Topic 1: Regular Strings

```csharp
// Escape sequences needed
string path = "C:\\Users\\Ben\\Documents";
string quote = "He said, \"Hello!\"";
string multiline = "Line 1\nLine 2\nLine 3";
```

### Topic 2: Verbatim Strings

```csharp
// @ makes it verbatim - no escaping!
string path = @"C:\Users\Ben\Documents";
string quote = @"He said, ""Hello!""";
string multiline = @"Line 1
Line 2
Line 3";  // Actual newlines!
```

