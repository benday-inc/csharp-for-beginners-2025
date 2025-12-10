# Code Samples for Title: C# From Scratch #19: Introduction to Multithreading

This file contains all code samples from the video.

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

