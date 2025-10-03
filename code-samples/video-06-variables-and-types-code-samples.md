# Code Samples for Title: Variables and Types in C#

This file contains all code samples from the video.

## Slide 3: Declaring Variables

```csharp
// Type variableName = value;
string name = "Ben";
int age = 42;
double salary = 75000.50;
bool isAwesome = true;

// You can also declare first, assign later
string city;
city = "Seattle";
```

## Slide 5: Choosing the Right Type

```csharp
// Age? Use int
int age = 25;

// Money? Use decimal for precision
decimal price = 19.99m;  // Note the 'm' suffix

// Scientific calculations? Use double
double distance = 384400.5;  // km to moon

// User input? Usually string first
string userInput = Console.ReadLine();
```

## Slide 6: Type Inference with 'var'

### Topic 1: Explicit Typing

```csharp
// Explicit
string message = "Hello";
int count = 42;
List<string> names = new List<string>();
```

### Topic 2: Type Inference

```csharp
// With var - compiler figures out the type
var message = "Hello";        // string
var count = 42;               // int
var names = new List<string>(); // List<string>
```

## Slide 7: Constants - Values That Never Change

```csharp
// Constants can't be changed after declaration
const double PI = 3.14159;
const int MAX_RETRY_COUNT = 3;
const string COMPANY_NAME = "Contoso";

// This won't compile:
// PI = 3.14;  // Error: Can't assign to const
```

## Slide 11: Value vs Reference in Action

### Topic 1: Value Type Behavior

```csharp
int a = 5;
int b = a;  // Copies the value 5
b = 10;
// a is still 5, b is 10
```

### Topic 2: Reference Type Behavior

```csharp
int[] a = new int[] {5};
int[] b = a;  // Copies the reference
b[0] = 10;
// Both a[0] and b[0] are now 10!
```

## Slide 12: Nullable Types

```csharp
// Regular int can't be null
int age = 25;
// age = null;  // Won't compile!

// Nullable int can be null
int? maybeAge = null;
maybeAge = 25;  // Can also hold a value

// Check before using
if (maybeAge.HasValue)
{
    Console.WriteLine($"Age is {maybeAge.Value}");
}
```

## Slide 13: Type Conversion

```csharp
// Implicit conversion (safe)
int count = 100;
double bigNumber = count;  // int fits in double

// Explicit conversion (casting)
double pi = 3.14159;
int roughPi = (int)pi;  // Loses decimal part!

// Parsing strings
string input = "42";
int parsed = int.Parse(input);  // Or TryParse for safety
```

