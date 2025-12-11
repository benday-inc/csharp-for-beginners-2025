# Code Samples, Tables, and Diagrams for: Debugging - Finding and Fixing Your Code Problems

This file contains code samples, tables, and diagrams from the video.

## Slide 3: The Error Message Is Your Friend

```text
Unhandled exception. System.NullReferenceException: 
Object reference not set to an instance of an object.
   at Program.<Main>$(String[] args) in 
   C:\MyProject\Program.cs:line 5
```

## Slide 5: Console.WriteLine: The Classic Debug

```csharp
int[] numbers = {1, 2, 3};
int sum = 0;

for(int i = 0; i <= numbers.Length; i++)  // Bug here!
{
    Console.WriteLine($"i = {i}, sum = {sum}");
    sum += numbers[i];  // This will crash when i = 3
}

Console.WriteLine($"Final sum: {sum}");
```

## Slide 6: Strategic Console.WriteLine Placement

```csharp
public string ProcessName(string name)
{
    Console.WriteLine($"Input: '{name}'");  // What came in?
    
    if (string.IsNullOrEmpty(name))
    {
        Console.WriteLine("Name was null or empty");
        return "Unknown";
    }
    
    string result = name.Trim().ToUpper();
    Console.WriteLine($"Output: '{result}'");  // What's going out?
    
    return result;
}
```

## Slide 13: The Debug Controls

```text
F5  - Start/Continue debugging
F9  - Toggle breakpoint on current line
F10 - Step Over (execute current line)
F11 - Step Into (go into method calls)
Shift+F11 - Step Out (exit current method)
Shift+F5 - Stop debugging
```

## Slide 14: Conditional Breakpoints

```csharp
for(int i = 0; i < 1000; i++)
{
    // Right-click breakpoint here
    // Add condition: i == 500
    ProcessItem(items[i]);
}

// Breakpoint only triggers when i equals 500
// No need to click Continue 499 times!
```

