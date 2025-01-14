# 14. OOP Structs and Records
Il s'agit d'un chapitre assez court, mais qui est malgré tout important. Il est nécessaire que vous sachiez qu'il co-existe avec des classes et des interfaces, des structures et des records. Voyons de quoi il s'agit, à quoi ils servent, tout cela bien entendu par des exemples.

## Structs
In C#, structures (or structs) are value types that allow you to encapsulate data and related functionalities. They are often used to represent lightweight concepts such as points in a 2D or 3D space, sizes, ranges of values, and other data that is frequently copied without modifications.

### Characteristics of structs
1. **Value Type**: Unlike classes, which are reference types, structs are value types. This means that when a structure is assigned to a new variable or passed to a method, a complete copy of it is made.

2. **Storage**: Structs are typically stored on the stack, which can offer performance benefits, particularly in reducing garbage collection pressure.

3. **Immutability**: Although structs can be mutable, it is advisable to make them immutable. An immutable structure does not allow its data to be modified after it is created, making the code safer and easier to understand.

4. **Inheritance**: Structures cannot inherit from other structures or classes but can implement interfaces.

5. **Initialization**: Every field in a structure must be initialized before the structure instance can be used.

### Using structs
Structs are particularly useful when you need to create small data objects that will not be modified after their creation. Here are some tips on when to use a structure:

- Use structs for small data structures.
- Favor structs when you need instances of data to be copied rather than referenced.
- Structs are efficient for passing data through method calls when you know there will be no modifications to the data.

### Example of a structure in C#

Here's an example of a structure representing a point in a two-dimensional coordinate system

```csharp
public struct Point
{
    public int X { get; }
    public int Y { get; }

    public Point(int x, int y)
    {
        X = x;
        Y = y;
    }

    public override string ToString()
    {
        return $"({X}, {Y})";
    }
}
```

In this example, `Point` is an immutable structure with read-only properties `X` and `Y`. Once a point is created with specific values for `X `and `Y`, these values cannot be changed.

### Best Practices
- Make structures immutable whenever possible.
- Implement interfaces if necessary to add specific functionalities.
- Use structs for data that is often copied but rarely modified after creation.

Structures are a powerful tool in C# for modeling simple and lightweight concepts. Proper use can make your code both faster and easier to maintain.

## Records
Introduced in C# 9, records are reference types designed to be immutable by default. They are particularly useful for creating data objects whose values are not supposed to change after they are created.

### Characteristics of Records

1. **Reference Type**: Records are reference types, but they are intended to provide simpler syntax and built-in behaviors for data modeling.

2. **Immutability**: By default, records are immutable. However, mutable records can be created by using the data keyword with properties.

3. **Value-based Equality**: Records support value-based equality out of the box. This means two records with the same values for all their properties are considered equal, unlike classes which compare reference identities by default.

4. **With Expressions**: Records support with expressions, allowing you to create a new record instance by copying an existing one with some properties modified, which is useful for creating modified copies of immutable objects.

5. **Deconstruction**: Records can be easily deconstructed into variables, similar to tuples, making them very convenient for returning multiple values from methods.

6. **Inheritance**: Records can inherit from other records. This provides a way to create hierarchies, which is not possible with structs. When a derived record type is compared for equality, the comparison includes the data from its base type.

### Using Records
Records are ideal for scenarios where you need to ensure that data objects are immutable and where you rely on value-based equality for comparison. Here are some typical use cases:

- Data transfer objects (DTOs) that carry data between processes.
- Domain entities where an immutable state is necessary.
- When you require a concise and expressive syntax for data modeling.

### Example of a Record in C#
Here’s how you might define a record for a person in C#:

```csharp
public record Person(string FirstName, string LastName);

public record Person
{
    public string FirstName { get; init; }
    public string LastName { get; init; }

    public Person(string firstName, string lastName) => (FirstName, LastName) = (firstName, lastName);

    public void Deconstruct(out string firstName, out string lastName) => (firstName, lastName) = (FirstName, LastName);
}
```

### Best Practices

- Use records when you need immutable data structures.
- Consider using records for types that benefit from value-based equality.
- Take advantage of records' built-in features like with expressions and deconstructors to simplify your code.

Records provide a modern way to define data structures in C# that are concise, immutable, and easy to use, making them a valuable addition to the language for managing data integrity and simplicity in complex applications.