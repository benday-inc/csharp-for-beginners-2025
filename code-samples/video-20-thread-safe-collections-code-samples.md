# Code Samples for Title: Thread-Safe Collections

This file contains all code samples from the video.

## Slide 3: List<T> Under Pressure

```csharp
var list = new List<int>();

Parallel.For(0, 10000, i =>
{
    list.Add(i);  // Multiple threads adding
});

Console.WriteLine($"Count: {list.Count}");
// Expected: 10,000
// Actual: 9,847 (or worse, an exception!)
```

## Slide 5: ConcurrentDictionary Basics

```csharp
var cache = new ConcurrentDictionary<string, int>();

// Adding - use TryAdd, not Add
cache.TryAdd("key1", 100);  // Returns true if added
cache.TryAdd("key1", 200);  // Returns false, already exists

// Reading - use TryGetValue
if (cache.TryGetValue("key1", out int value))
    Console.WriteLine(value);  // 100

// Removing
cache.TryRemove("key1", out _);  // Returns true if removed
```

## Slide 6: GetOrAdd - The Atomic Pattern

### Topic 1: Race Condition Risk

```csharp
// DON'T DO THIS
if (!cache.ContainsKey("key"))
{
    // Another thread adds "key" here!
    cache.TryAdd("key", ComputeValue());
}
var value = cache["key"];
// ComputeValue() ran twice!
```

### Topic 2: Thread-Safe

```csharp
// DO THIS
var value = cache.GetOrAdd(
    "key",
    key => ComputeValue()
);
// ComputeValue() runs only once
// (usually - see next slide)
```

## Slide 7: GetOrAdd - Caching Pattern

```csharp
public class UserCache
{
    private readonly ConcurrentDictionary<int, User> _cache = new();
    private readonly IUserRepository _repo;

    public User GetUser(int userId)
    {
        return _cache.GetOrAdd(
            userId,
            id => _repo.LoadUser(id)  // Only called if not cached
        );
    }

    public void InvalidateUser(int userId)
    {
        _cache.TryRemove(userId, out _);
    }
}
```

## Slide 8: AddOrUpdate - Modify Atomically

```csharp
var scores = new ConcurrentDictionary<string, int>();

// Add 10 points for "player1"
scores.AddOrUpdate(
    "player1",
    addValue: 10,           // If key doesn't exist, add 10
    updateValueFactory: (key, oldValue) => oldValue + 10
);                          // If exists, add 10 to current

// Or with factory functions for both:
scores.AddOrUpdate(
    "player1",
    key => LoadInitialScore(key),
    (key, oldValue) => oldValue + 10
);
```

## Slide 9: ConcurrentBag - Unordered and Fast

```csharp
var bag = new ConcurrentBag<WorkItem>();

// Multiple threads can add simultaneously
Parallel.For(0, 1000, i =>
{
    bag.Add(new WorkItem(i));
});

Console.WriteLine($"Count: {bag.Count}");  // Exactly 1000!

// Take items out (order not guaranteed)
while (bag.TryTake(out var item))
{
    ProcessItem(item);
}
```

## Slide 10: ConcurrentQueue and ConcurrentStack

### Topic 1: ConcurrentQueue (FIFO)

```csharp
var queue = new ConcurrentQueue<string>();

queue.Enqueue("first");
queue.Enqueue("second");
queue.Enqueue("third");

while (queue.TryDequeue(out var item))
    Console.WriteLine(item);
// first, second, third
```

### Topic 2: ConcurrentStack (LIFO)

```csharp
var stack = new ConcurrentStack<string>();

stack.Push("first");
stack.Push("second");
stack.Push("third");

while (stack.TryPop(out var item))
    Console.WriteLine(item);
// third, second, first
```

## Slide 11: BlockingCollection - Producer/Consumer

```csharp
var collection = new BlockingCollection<WorkItem>(boundedCapacity: 100);

// Producer thread
Task.Run(() =>
{
    foreach (var item in GetWorkItems())
        collection.Add(item);  // Blocks if full!
    collection.CompleteAdding();
});

// Consumer thread
Task.Run(() =>
{
    foreach (var item in collection.GetConsumingEnumerable())
        ProcessItem(item);  // Blocks if empty!
});
```

## Slide 12: Complete Producer-Consumer Example

```csharp
var files = new BlockingCollection<string>(boundedCapacity: 50);

// Producer: finds files
var producer = Task.Run(() =>
{
    foreach (var file in Directory.EnumerateFiles(path, "*.csv"))
        files.Add(file);
    files.CompleteAdding();
});

// Multiple consumers: process files
var consumers = Enumerable.Range(0, 4).Select(_ => Task.Run(() =>
{
    foreach (var file in files.GetConsumingEnumerable())
        ProcessFile(file);
})).ToArray();

await Task.WhenAll(consumers);
```

## Slide 14: Lazy Factory Pattern

### Topic 1: Problem: Multiple Calls

```csharp
// Factory might run multiple times!
var user = cache.GetOrAdd(
    userId,
    id => LoadUserFromDb(id)
);
// LoadUserFromDb could be called
// 2-3 times under contention
```

### Topic 2: Solution: Lazy<T>

```csharp
// Lazy ensures single execution
var lazyUser = cache.GetOrAdd(
    userId,
    id => new Lazy<User>(
        () => LoadUserFromDb(id))
);
var user = lazyUser.Value;
// LoadUserFromDb runs exactly once
```

