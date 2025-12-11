# Code Samples, Tables, and Diagrams for: Tasks - Modern Multithreading

This file contains code samples, tables, and diagrams from the video.

## Slide 3: Thread vs Task Creation

### Topic 1: Raw Thread (Old Way)

```csharp
// Creates a NEW thread every time
var thread = new Thread(() =>
{
    DoExpensiveWork();
});
thread.Start();

// No easy way to:
// - Get a return value
// - Know when it's done
// - Handle exceptions
```

### Topic 2: Task (Modern Way)

```csharp
// Uses thread pool - much cheaper
Task task = Task.Run(() =>
{
    DoExpensiveWork();
});

// Easy to:
await task;           // Wait for completion
var result = await t; // Get return value
// Exceptions propagate naturally
```

## Slide 4: Your First Task

```csharp
Console.WriteLine($"Main thread: {Thread.CurrentThread.ManagedThreadId}");

Task task = Task.Run(() =>
{
    Console.WriteLine($"Task thread: {Thread.CurrentThread.ManagedThreadId}");
    Thread.Sleep(2000);  // Simulate work
    Console.WriteLine("Task complete!");
});

Console.WriteLine("Main continues immediately...");
await task;  // Wait for task to finish
Console.WriteLine("All done!");
```

## Slide 5: Tasks That Return Values

```csharp
// Task<int> returns an int when complete
Task<int> task = Task.Run(() =>
{
    int sum = 0;
    for (int i = 0; i < 1_000_000; i++)
        sum += i;
    return sum;  // This becomes the Task's result
});

// Do other work while sum is being calculated...

int result = await task;  // Get the computed value
Console.WriteLine($"Sum: {result}");
```

## Slide 6: Waiting for Tasks

| Method | Blocks Thread? | When to Use |
| :--- | :--- | :--- |
| await task | No ✅ | Always prefer this in async methods |
| task.Wait() | Yes ⚠️ | Only in Main() or top-level code |
| task.Result | Yes ⚠️ | Same as Wait(), blocks until done |
| Task.WaitAll() | Yes ⚠️ | Wait for multiple tasks (blocking) |
| Task.WhenAll() | No ✅ | Await multiple tasks (non-blocking) |

## Slide 7: Running Multiple Tasks

```csharp
// Start three tasks in parallel
var task1 = Task.Run(() => DownloadFile("file1.txt"));
var task2 = Task.Run(() => DownloadFile("file2.txt"));
var task3 = Task.Run(() => DownloadFile("file3.txt"));

// Wait for ALL to complete (non-blocking)
await Task.WhenAll(task1, task2, task3);

// Or get results from all:
var tasks = new[] { task1, task2, task3 };
string[] results = await Task.WhenAll(tasks);
```

## Slide 8: Task.WhenAny - First One Wins

```csharp
// Try multiple servers, use first response
var tasks = new[]
{
    FetchFromServer("server1.com"),
    FetchFromServer("server2.com"),
    FetchFromServer("server3.com")
};

Task<string> winner = await Task.WhenAny(tasks);
string result = await winner;  // Get the actual result

Console.WriteLine($"Got response: {result}");
// Other tasks continue running (or you can cancel them)
```

## Slide 9: Exception Handling in Tasks

```csharp
Task task = Task.Run(() =>
{
    throw new InvalidOperationException("Something broke!");
});

// Exception doesn't throw here!
Console.WriteLine("Task is running...");

try
{
    await task;  // Exception throws HERE
}
catch (InvalidOperationException ex)
{
    Console.WriteLine($"Caught: {ex.Message}");
}
```

## Slide 10: AggregateException - Multiple Failures

```csharp
var tasks = new[]
{
    Task.Run(() => { throw new Exception("Task 1 failed"); }),
    Task.Run(() => { throw new Exception("Task 2 failed"); }),
    Task.Run(() => "Task 3 succeeded")
};

try
{
    await Task.WhenAll(tasks);
}
catch (Exception ex)
{
    // ex is the FIRST exception
    // For ALL exceptions:
    var allExceptions = tasks
        .Where(t => t.IsFaulted)
        .SelectMany(t => t.Exception.InnerExceptions);
}
```

## Slide 12: Async Lambdas Trap

### Topic 1: Hidden Problem

```csharp
// WRONG: async void lambda!
Task.Run(async () =>
{
    await SomeAsyncWork();
    throw new Exception("Oops!");
});
// Exception is LOST!
// Task.Run returns immediately
```

### Topic 2: Correct Approach

```csharp
// CORRECT: async Task lambda
Task task = Task.Run(async () =>
{
    await SomeAsyncWork();
    throw new Exception("Oops!");
});
await task;  // Exception throws here
```

## Slide 15: Practical Pattern: Timeout

```csharp
async Task<T> WithTimeout<T>(Task<T> task, TimeSpan timeout)
{
    var delayTask = Task.Delay(timeout);
    var winner = await Task.WhenAny(task, delayTask);
    
    if (winner == delayTask)
        throw new TimeoutException("Operation timed out");
    
    return await task;  // Get the actual result
}

// Usage:
var result = await WithTimeout(
    SlowOperation(), 
    TimeSpan.FromSeconds(5));
```

## Slide 16: Practical Pattern: Progress Reporting

```csharp
async Task ProcessFilesAsync(IProgress<int> progress)
{
    var files = Directory.GetFiles(path);
    for (int i = 0; i < files.Length; i++)
    {
        await ProcessFileAsync(files[i]);
        progress.Report((i + 1) * 100 / files.Length);
    }
}

// Usage:
var progress = new Progress<int>(percent =>
    Console.WriteLine($"Progress: {percent}%"));

await ProcessFilesAsync(progress);
```

