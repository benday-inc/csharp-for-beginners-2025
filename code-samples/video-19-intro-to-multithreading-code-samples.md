# Code Samples, Tables, and Diagrams for: C# From Scratch #19: Introduction to Multithreading

This file contains code samples, tables, and diagrams from the video.

## Slide 3: Inspecting Threads

```csharp
using System.Diagnostics;
using System.Threading;

var thread = Thread.CurrentThread;
Console.WriteLine($"ID: {thread.ManagedThreadId}");
Console.WriteLine($"Priority: {thread.Priority}");

var process = Process.GetCurrentProcess();
Console.WriteLine($"Total threads: {process.Threads.Count}");
```

## Slide 4: Creating a Thread (The Old Way)

```csharp
void DoWork()
{
    for (int i = 0; i < 5; i++)
    {
        Console.WriteLine($"Worker: {i}");
        Thread.Sleep(1000);
    }
}

Thread worker = new Thread(DoWork);
worker.Start();

for (int i = 0; i < 5; i++)
    Console.WriteLine($"Main: {i}");
```

## Slide 6: Race Condition - The Bug That Hides

```csharp
int counter = 0;

void IncrementCounter()
{
    for (int i = 0; i < 100000; i++)
        counter++;  // Looks safe, right?
}

var t1 = new Thread(IncrementCounter);
var t2 = new Thread(IncrementCounter);
t1.Start(); t2.Start();
t1.Join(); t2.Join();  // Wait for both

Console.WriteLine($"Counter: {counter}");
// Expected: 200,000. Actual: 137,421 (?!?)
```

## Slide 7: How Lost Updates Happen

### Source Data

| Step | Thread 1 | Thread 2 | Counter | Problem? |
| :--- | :--- | :--- | :--- | :--- |
| 1 | Read: 100 | - | 100 |  |
| 2 | Add 1: 101 | Read: 100 | 100 | ⚠ |
| 3 | Write: 101 | Add 1: 101 | 101 |  |
| 4 | - | Write: 101 | 101 | ❌ |
| 5 | Read: 101 | - | 101 |  |
| 6 | Add 1: 102 | - | 101 |  |
| 7 | Write: 102 | - | 102 |  |

### Query

```text
// What we expect:
counter++;  // 100 → 101
counter++;  // 101 → 102

// What actually happens:
// Both read 100 before either writes!
// One increment is LOST
```

### Result

| Operation | Expected | Actual |
| :--- | :--- | :--- |
| Two increments | 100 → 102 | 100 → 101 |
| Lost updates | 0 | 1 |
| Final count (100k each) | 200,000 | ~150,000 |

## Slide 8: The Solution: Lock Statement

### Topic 1: Broken

```csharp
int counter = 0;

void Increment()
{
    for (int i = 0; i < 100000; i++)
        counter++;
}
// Race condition!
```

### Topic 2: Fixed

```csharp
int counter = 0;
object lockObj = new object();

void Increment()
{
    for (int i = 0; i < 100000; i++)
        lock (lockObj) { counter++; }
}
// Thread-safe!
```

## Slide 9: Interlocked - Lock-Free Speed

```csharp
int counter = 0;

void IncrementCounter()
{
    for (int i = 0; i < 100000; i++)
        Interlocked.Increment(ref counter);
}

// Also available:
Interlocked.Decrement(ref counter);
Interlocked.Add(ref counter, 5);
Interlocked.CompareExchange(ref val, newVal, expected);
```

## Slide 10: What's Actually Thread-Safe?

| Operation | Thread-Safe? | Notes |
| :--- | :--- | :--- |
| int x = 5; | ✅ Yes | Single CPU instruction |
| object x = y; | ✅ Yes | Reference copy is atomic |
| x++, x += 1 | ❌ No | Read-modify-write! |
| List.Add() | ❌ No | Most class methods |
| Immutable types | ✅ Yes | Can't change = safe |
| Local variables | ✅ Yes | Each thread gets own |
| Static variables | ❌ No | Shared across threads |

## Slide 11: Deadlock - When Locks Lock Up

```csharp
object lock1 = new object();
object lock2 = new object();

void Method1(){
    lock (lock1) {
        Thread.Sleep(100);
        lock (lock2) { DoWork(); } // Waits for lock2
    }
}
void Method2(){
    lock (lock2) {
        Thread.Sleep(100);
        lock (lock1) { DoWork(); } // Waits for lock1
    }
}
// Both threads frozen forever!
```

## Slide 14: Thread-Safe Collections Preview

| Regular | Thread-Safe | Use Case |
| :--- | :--- | :--- |
| List<T> | ConcurrentBag<T> | Unordered additions |
| Queue<T> | ConcurrentQueue<T> | Producer-consumer |
| Stack<T> | ConcurrentStack<T> | LIFO operations |
| Dictionary<K,V> | ConcurrentDictionary<K,V> | Shared lookups |
| N/A | BlockingCollection<T> | Bounded buffer |

