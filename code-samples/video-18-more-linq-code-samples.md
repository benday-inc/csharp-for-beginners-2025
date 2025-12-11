# Code Samples, Tables, and Diagrams for: C# From Scratch #18: More LINQ Power

This file contains code samples, tables, and diagrams from the video.

## Slide 2: GroupBy - The Game Changer

```csharp
// Group orders by customer
var customerOrders = orders
    .GroupBy(o => o.CustomerId)
    .Select(g => new 
    {
        CustomerId = g.Key,
        OrderCount = g.Count(),
        TotalSpent = g.Sum(o => o.Amount)
    });
```

## Slide 3: GroupBy Visual Example

### Source Data

| Order# | Customer | Amount |
| :--- | :--- | ---: |
| 001 | Alice | $50 |
| 002 | Bob | $30 |
| 003 | Alice | $75 |
| 004 | Charlie | $100 |
| 005 | Bob | $45 |
| 006 | Alice | $25 |

### Query

```linq
orders
  .GroupBy(o => o.Customer)
  .Select(g => new {
    Customer = g.Key,
    OrderCount = g.Count(),
    Total = g.Sum(o => o.Amount)
  })
```

### Result

| Customer | OrderCount | Total |
| :--- | ---: | ---: |
| Alice | 3 | $150 |
| Bob | 2 | $75 |
| Charlie | 1 | $100 |

## Slide 4: Join - Combining Two Datasets

```csharp
// Match customers with their orders
var customerOrders = customers
    .Join(orders,
        c => c.Id,           // Key from customers
        o => o.CustomerId,   // Key from orders
        (c, o) => new        // Result selector
        {
            c.Name,
            o.OrderDate,
            o.Amount
        });
```

## Slide 5: Join Visual Example

### Source Data

| CustId | Name | OrderId | CustId | Amount |
| :--- | :--- | :--- | :--- | ---: |
| 1 | Alice | A01 | 1 | $50 |
| 2 | Bob | A02 | 2 | $30 |
| 3 | Carol | A03 | 1 | $75 |

### Query

```linq
customers.Join(orders,
  c => c.CustId,
  o => o.CustId,
  (c, o) => new {
    c.Name, o.OrderId, o.Amount
  })
```

### Result

| Name | OrderId | Amount |
| :--- | :--- | ---: |
| Alice | A01 | $50 |
| Alice | A03 | $75 |
| Bob | A02 | $30 |

## Slide 6: OrderBy and ThenBy - Multi-Level Sorting

```csharp
// Multi-level sorting
var sorted = employees
    .OrderBy(e => e.Department)
    .ThenBy(e => e.LastName)
    .ThenBy(e => e.FirstName);

// Descending variations
var topEarners = employees
    .OrderByDescending(e => e.Salary)
    .ThenBy(e => e.HireDate);  // Ties by seniority
```

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

## Slide 8: Aggregate Functions - Sum, Count, Average

```csharp
var orders = GetOrders();

// Basic aggregates
var totalRevenue = orders.Sum(o => o.Amount);
var orderCount = orders.Count();
var averageOrder = orders.Average(o => o.Amount);
var largestOrder = orders.Max(o => o.Amount);
var smallestOrder = orders.Min(o => o.Amount);

// Conditional counting
var bigOrders = orders.Count(o => o.Amount > 100);
```

## Slide 9: GroupBy with Aggregates

### Source Data

| Product | Category | Price | Qty |
| :--- | :--- | ---: | ---: |
| Widget | Hardware | $10 | 5 |
| Gadget | Hardware | $25 | 3 |
| Gizmo | Software | $50 | 2 |
| Tool | Hardware | $15 | 8 |
| App | Software | $30 | 4 |

### Query

```linq
products
  .GroupBy(p => p.Category)
  .Select(g => new {
    Category = g.Key,
    ProductCount = g.Count(),
    TotalRevenue = g.Sum(
      p => p.Price * p.Qty),
    AvgPrice = g.Average(p => p.Price)
  })
```

### Result

| Category | Products | Revenue | AvgPrice |
| :--- | ---: | ---: | ---: |
| Hardware | 3 | $245 | $16.67 |
| Software | 2 | $220 | $40.00 |

## Slide 11: Deferred vs Immediate Execution

| Method | Execution | Returns |
| :--- | :--- | :--- |
| Where, Select, OrderBy | Deferred | IEnumerable<T> (query) |
| ToList, ToArray | Immediate | List<T> or T[] (data) |
| ToDictionary | Immediate | Dictionary<K,V> |
| First, Single, Last | Immediate | Single item T |
| Count, Sum, Average | Immediate | Single value |
| Any, All | Immediate | Boolean |
| Min, Max | Immediate | Single value |
| GroupBy | Deferred | IEnumerable<IGrouping> |
| Take, Skip | Deferred | IEnumerable<T> (query) |

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

## Slide 14: Real-World Example: Sales Dashboard

```csharp
// Get this month's sales - execute once!
var salesData = GetAllSales()
    .Where(s => s.Date.Month == DateTime.Now.Month)
    .ToList();  // Snapshot for reuse

// Top 10 products by revenue
var topProducts = salesData
    .GroupBy(s => s.ProductId)
    .Select(g => new
    {
        Product = g.First().ProductName,
        Units = g.Sum(s => s.Quantity),
        Revenue = g.Sum(s => s.Quantity * s.Price)
    })
    .OrderByDescending(p => p.Revenue)
    .Take(10);

// Regional performance
var byRegion = salesData
    .GroupBy(s => s.Region)
    .Select(g => new { 
        Region = g.Key, 
        Total = g.Sum(s => s.Amount) 
    })
    .OrderByDescending(r => r.Total);

// Quick metrics
var totalRevenue = salesData.Sum(s => s.Amount);
var avgSale = salesData.Average(s => s.Amount);
```

