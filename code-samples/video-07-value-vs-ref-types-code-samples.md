# Code Samples for Title: Value vs Reference Types Deep Dive

This file contains all code samples from the video.

## Slide 4: Value Types in Memory

```csharp
void MyMethod()
{
    int x = 42;      // On the stack
    int y = x;       // Copies the value 42
    y = 100;         // x is still 42
    
    // When MyMethod ends, x and y 
    // are immediately removed from stack
}
```

## Slide 5: Reference Types in Memory

```csharp
void MyMethod()
{
    string name = "Ben";     // "Ben" on heap, reference on stack
    string alias = name;     // Copies the reference
    
    Person p = new Person(); // Person object on heap
                            // p (the reference) on stack
}
```

## Slide 6: The Assignment Gotcha

### Topic 1: Value Type Assignment

```csharp
struct Point { public int X, Y; }

Point p1 = new Point { X = 5, Y = 10 };
Point p2 = p1;  // Copies entire struct
p2.X = 20;

// p1.X is still 5
// p2.X is 20
```

### Topic 2: Reference Type Assignment

```csharp
class Person { public string Name; }

Person p1 = new Person { Name = "Ben" };
Person p2 = p1;  // Copies reference only!
p2.Name = "Sarah";

// p1.Name is now "Sarah" too!
// Both point to same object
```

## Slide 7: Passing to Methods

### Topic 1: Value Parameter

```csharp
void DoubleIt(int num)
{
    num = num * 2;  // Only changes local copy
}

int x = 5;
DoubleIt(x);
// x is still 5
```

### Topic 2: Reference Parameter

```csharp
void ChangeName(Person p)
{
    p.Name = "Changed";  // Changes the object!
}

Person person = new Person { Name = "Ben" };
ChangeName(person);
// person.Name is now "Changed"
```

## Slide 8: The 'ref' Keyword

```csharp
void DoubleIt(ref int num)
{
    num = num * 2;  // Now changes the original!
}

int x = 5;
DoubleIt(ref x);  // Note: ref here too
// x is now 10

// Works with reference types too
void CreateNew(ref Person p)
{
    p = new Person();  // Changes what p points to
}
```

## Slide 11: The String Exception

```csharp
string s1 = "Hello";
string s2 = s1;
s2 = "World";  // Creates NEW string object

// s1 is still "Hello"
// s2 is "World"

// Even though string is reference type,
// it acts like value type due to immutability
```

## Slide 12: Boxing and Unboxing

```csharp
int value = 42;              // Value type on stack
object boxed = value;        // Boxing - copies to heap
int unboxed = (int)boxed;    // Unboxing - copies back

// Boxing is expensive! Avoid in loops
ArrayList list = new ArrayList();
for(int i = 0; i < 1000; i++)
{
    list.Add(i);  // Boxes every int!
}
// Use List<int> instead
```

