# Code Samples for Title: C# From Scratch #18: More LINQ Power

This file contains all code samples from the video.

## Slide 7: Don't do this with OrderBy!

### Topic 1: Wrong Way

```csharp
// DON'T DO THIS!
var wrong = employees
    .OrderBy(e => e.Department)
    .OrderBy(e => e.LastName);
// Second OrderBy REPLACES the first!
// Only sorted by LastName
```

### Topic 2: Right Way

```csharp
// DO THIS INSTEAD
var right = employees
    .OrderBy(e => e.Department)
    .ThenBy(e => e.LastName);
// Sorted by Department, then LastName
// Multi-level sorting works!
```

## Slide 12: The Multiple Enumeration Problem

### Topic 1: The Problem

```csharp
// BAD: Query runs TWICE!
var expensive = GetAllOrders()
    .Where(o => o.Amount > 100);

var count = expensive.Count();
// Query runs once ^

foreach (var order in expensive)
{
    // Query runs AGAIN!
}
```

### Topic 2: The Fix

```csharp
// GOOD: Query runs once
var expensive = GetAllOrders()
    .Where(o => o.Amount > 100)
    .ToList();  // Execute once!

var count = expensive.Count;
// Just reads the list

foreach (var order in expensive)
{
    // Just reads the list
}
```

