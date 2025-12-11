# Code Samples, Tables, and Diagrams for: Video 15: AppDomains and Assemblies

This file contains code samples, tables, and diagrams from the video.

## Slide 3: Inside an Assembly

```csharp
// When you write this:
namespace MyApp
{
    public class Customer
    {
        public string Name { get; set; }
        public void SendEmail() { }
    }
    
    public class Order
    {
        public decimal Total { get; set; }
    }
}

// It all goes into: MyApp.dll
```

## Slide 5: Assembly References

```xml
<!-- In your .csproj file -->
<Project>
  <ItemGroup>
    <!-- NuGet package reference -->
    <PackageReference Include="Newtonsoft.Json" Version="13.0.3" />
    
    <!-- Project reference -->
    <ProjectReference Include="..\MyLibrary\MyLibrary.csproj" />
    
    <!-- Direct DLL reference (rare now) -->
    <Reference Include="SomeLegacy.dll" />
  </ItemGroup>
</Project>
```

## Slide 7: Why AppDomains Mattered

```csharp
// Old .NET Framework way (doesn't work in .NET Core!)
// This is just to show you what it looked like

// Create a new AppDomain
AppDomain pluginDomain = AppDomain.CreateDomain("PluginDomain");

// Load an assembly into it
pluginDomain.Load("MyPlugin.dll");

// Later, unload everything
AppDomain.Unload(pluginDomain);
// The plugin and all its memory is gone!
```

## Slide 9: AssemblyLoadContext (The New Way)

```csharp
// Modern .NET Core/.NET 5+ way
using System.Runtime.Loader;

class PluginLoadContext : AssemblyLoadContext
{
    public PluginLoadContext() : base(isCollectible: true) { }
}

// Load a plugin
var context = new PluginLoadContext();
var assembly = context.LoadFromAssemblyPath("Plugin.dll");

// Later, unload it
context.Unload();
```

