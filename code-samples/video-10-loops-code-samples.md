# Code Samples for Title: Loops - Doing Things Multiple Times

This file contains all code samples from the video.

## Slide 3: The For Loop

```csharp
// Basic for loop
for (int i = 0; i < 5; i++)
{
    Console.WriteLine($"Count: {i}");
}
// Prints: 0, 1, 2, 3, 4

// Three parts:
// 1. Initialize: int i = 0
// 2. Condition: i < 5
// 3. Update: i++
```

## Slide 4: For Loop Variations

```csharp
// Count backwards
for (int i = 10; i > 0; i--)
{
    Console.WriteLine($"Countdown: {i}");
}

// Skip by 2
for (int i = 0; i < 100; i += 2)
{
    Console.WriteLine(i);  // Even numbers only
}

// Multiple variables
for (int i = 0, j = 10; i < j; i++, j--)
{
    Console.WriteLine($"i: {i}, j: {j}");
}
```

## Slide 5: The While Loop

```csharp
// Keep asking until valid input
string input = "";
while (input != "quit")
{
    Console.WriteLine("Enter command (or 'quit' to exit):");
    input = Console.ReadLine();
    Console.WriteLine($"You said: {input}");
}

// Process until condition met
int total = 0;
while (total < 100)
{
    total += Random.Next(1, 20);
    Console.WriteLine($"Total: {total}");
}
```

## Slide 6: Do-While: Execute at Least Once

### Topic 1: While Loop

```csharp
// Might never execute
int x = 10;
while (x < 5)
{
    Console.WriteLine("This won't print");
    x++;
}
```

### Topic 2: Do-While Loop

```csharp
// Always executes at least once
int x = 10;
do
{
    Console.WriteLine("This WILL print once");
    x++;
} while (x < 5);
```

## Slide 7: The Foreach Loop

```csharp
// Iterate through array
int[] numbers = {10, 20, 30, 40, 50};
foreach (int num in numbers)
{
    Console.WriteLine($"Value: {num}");
}

// Iterate through list
List<string> names = new List<string> {"Alice", "Bob", "Charlie"};
foreach (string name in names)
{
    Console.WriteLine($"Hello, {name}!");
}

// You can't modify the collection while iterating!
```

## Slide 9: Break and Continue

```csharp
// Break - exit the loop early
for (int i = 0; i < 100; i++)
{
    if (i == 10)
    {
        break;  // Stop at 10
    }
    Console.WriteLine(i);
}

// Continue - skip to next iteration
for (int i = 0; i < 10; i++)
{
    if (i % 2 == 0)
    {
        continue;  // Skip even numbers
    }
    Console.WriteLine(i);  // Only odd numbers print
}
```

## Slide 10: Nested Loops

```csharp
// Multiplication table
for (int i = 1; i <= 5; i++)
{
    for (int j = 1; j <= 5; j++)
    {
        Console.Write($"{i * j,4}");
    }
    Console.WriteLine();  // New line after each row
}

// Output:
//    1   2   3   4   5
//    2   4   6   8  10
//    3   6   9  12  15
// etc...
```

## Slide 14: Common Loop Patterns

```csharp
// Finding an item
string target = "Alice";
bool found = false;
foreach (string name in names)
{
    if (name == target)
    {
        found = true;
        break;  // Stop once found
    }
}

// Accumulating values
int sum = 0;
foreach (int value in numbers)
{
    sum += value;
}

// Filtering
List<int> evens = new List<int>();
foreach (int num in numbers)
{
    if (num % 2 == 0)
        evens.Add(num);
}
```

