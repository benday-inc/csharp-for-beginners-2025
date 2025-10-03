# Code Samples for Title: Understanding Project Structure

This file contains all code samples from the video.

## Slide 4: The Anatomy of a .csproj File

```xml
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>net8.0</TargetFramework>
    <Nullable>enable</Nullable>
    <ImplicitUsings>enable</ImplicitUsings>
  </PropertyGroup>

</Project>
```

## Slide 7: Creating a Solution Structure

```bash
# Create a solution
dotnet new sln -n MyApp

# Create projects
dotnet new console -n MyApp.Console
dotnet new classlib -n MyApp.Core

# Add projects to solution
dotnet sln add MyApp.Console/MyApp.Console.csproj
dotnet sln add MyApp.Core/MyApp.Core.csproj
```

## Slide 9: Project References

### Topic 1: Without Reference

```xml
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <TargetFramework>net8.0</TargetFramework>
  </PropertyGroup>
</Project>
```

### Topic 2: With Project Reference

```xml
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <TargetFramework>net8.0</TargetFramework>
  </PropertyGroup>
  
  <ItemGroup>
    <ProjectReference Include="..\MyApp.Core\MyApp.Core.csproj" />
  </ItemGroup>
</Project>
```

## Slide 10: Package References (NuGet)

```xml
<ItemGroup>
  <PackageReference Include="Newtonsoft.Json" Version="13.0.3" />
  <PackageReference Include="Dapper" Version="2.1.35" />
</ItemGroup>
```

