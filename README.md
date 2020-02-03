# C# Notes

## Configure C# language version on `.csporject` dotnet-core

```xml
<PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>netcoreapp3.1</TargetFramework>
    <LangVersion>8</LangVersion>
</PropertyGroup>
```

## ConfigureAwait(false)

As a general rule, every piece of code that is not in a view model and/or that does not need to go back on the main thread should use ConfigureAwait false.

This is simple, easy and can improve the performance of an application by freeing the UI thread for a little longer.

It is not only a matter or performance but also a matter of avoiding potential deadlocks.

## Ranges and Indices

```c#
string[] names = { "Dirk", "Jane", "James", "Albert", "Sally" };
foreach (var name in names[1..4])
{
    // do something
}

Range range = 1..4;
foreach (var name in names[range])
{
    // do something
}
```

1. The expression ..^1 is the same as 0..^1
2. The expression 1.. is the same as 1..^0
3. The expression .. is the same as 0..^0

## Recursive Patterns

```c#
foreach (var person in personList)
{
    // if person is Person type and RegisteredToVote property is false
    if (person is Person { RegisteredToVote: false })
    {
        WriteLine($"{person.Name} has not registered.");
    }
}
```

## Switch Expressions

```c#
var result = species switch
{
    Mammal m => $"{m.Name} is a {nameof(Mammal)}",
    Reptile r => $"{r.Name} is a {nameof(Reptile)}",
    _ => "Species could not be determined"
};
WriteLine(result);

var result = species switch
{
    Mammal m => $"{m.Name} is a {nameof(Mammal)}",
    Reptile r => $"{r.Name} is a {nameof(Reptile)}",
    {} => species.ToString(), // means not null
    null => "null"
};
WriteLine(result);
```

### var declarations in case expressions

```c#
case var o when (o?.Trim().Length ?? 0) == 0: // white space
    return null;
```

```c#
Reptile {isTrue: t} r  => $"{r.Name} is a {t} thats isTrue",
```

### Property Patterns

```c#
Reptile r when r.isTrue => $"{r.Name} is a {nameof(Reptile)} thats isTrue",
```

```c#
Reptile {isTrue: true} r  => $"{r.Name} is a {nameof(Reptile)} thats isTrue",
```

## Target-Typed New Expressions

```c#
Point[] ps = { new (1, 4), new (3, 2), new (9, 5) };
```

## Async Streams

```c#
static IAsyncEnumerable<int> GetLotsAsync()
{
    await foreach(var item in GetSomethingAsync())
    {
        if (item > 8)
        yield return item;
    }
}
```

#### OBSERVABLES VS. ASYNC STREAMS

With observables, the producer decides the timing of the data being delivered to the consumer.
In async streams the consumer decides when it’s ready to receive the data.

## Using Declarations

```c#
using var con = new SqlConnection(sqlConnStr);
SqlCommand cmd = new SqlCommand(tsql, con);
```

## ??= operators

```c#
int? str = null;
int b = str ??= 1;
Console.WriteLine(str); // 1
Console.WriteLine(b);  // 1
```

## ! (null-forgiving) operator

Without the null-forgiving operator, the compiler generates the following warning for the preceding code: Warning CS8625: Cannot convert null literal to non-nullable reference type. By using the null-forgiving operator, you inform the compiler that passing null is expected and shouldn't be warned about.

```c#
#nullable enable
public class Person
{
    public Person(string name) => Name = name ?? throw new ArgumentNullException(nameof(name));
    public string Name { get; }
}

public void NullNameShouldThrowTest()
{
    var person = new Person(null!);
}
```

```c#
#nullable enable
class NullableExpression
{
    public static void Run()
    {
        string? s = null;
        var l = s!.Length;
        Console.WriteLine(l);
    }
}
```
