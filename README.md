# 14. OOP Structs and Records

Before delving into the subject of this chapter, it is important to understand memory management for your future applications.

![](https://csharpforbeginners.wordpress.com/wp-content/uploads/2012/06/stack-heap.jpg)
https://csharpforbeginners.wordpress.com/2012/06/14/understanding-stack-and-heap-1/

## 1. Stack VS Heap
In C#, as in many programming languages, memory is divided into two main areas:

- The stack
- The heap

These two areas are used to store different kinds of data and operate in distinct ways. Let's take a closer look.

### 1. The stack
The stack is a fast memory area automatically managed by the system. It is used to store :

- Local variables (declared in a method or code block).
- Function parameters.
- Return pointers (execution address after a method call).

#### Stack characteristics:

- Works like a stack of books: last in, first out (LIFO - Last In, First Out).
- Automatic allocation/deallocation (when a method is called, memory is allocated; when it ends, it is freed).
- Very fast, but limited in size.

### 2. The heap
The heap is a larger but slower memory area than the stack. It is used to store :
    - Objects (class instances).
    - Reference types.

#### Heap features:
- Dynamic allocation (managed by the Garbage Collector in C#).
- Memory remains allocated as long as it is referenced by a variable.
- Slower than the heap, as the system has to manage memory and reclaim unused space.

### 3. Difference between value and reference types
C# distinguishes between two main families of types:
- **Value types (stored on the stack)**: int, double, bool, char, struct, enum.
- **Reference types (stored in the heap)**: class, interface, string, array, object.

### 4. Example
Theory is good, but with an example, it is better

```csharp
void Main()
{
    int a = 5;
    int b = a; 

    Personn p1 = new Personn(); 
    p1.Name = "Jean";
    
    Personn p2 = p1; 
    
    Console.WriteLine(p1.Name); // Jean
    Console.WriteLine(p2.Name); // Jean

    p2.Name = "Marie";
    
    Console.WriteLine(p1.Name); 
}

class Personn
{
    public string Name;
}
```

Wait, I don't understand .... don't worry

```csharp
int a = 5;
int b = a; 
```

- `a` is a variable of type `int`, which is a value type. It is stored in the **stack**.
- `b` = a; creates an independent copy of `a`. Each has its own value in memory.

| Stack            | Heap         
| :--------------- |:---------------:| 
| a = 5            |                 |
| b = a (5)        |                 |

- Changing `b` will not affect `a`, as they are totally independent.

---

```csharp
Personn p1 = new Personn();
p1.Nom = "Jean";
```

- `Personn` is a class, and therefore a `reference type`.
- The instruction `new Personne()` creates an object in the **heap**.
- `p1` is a reference (pointer) stored on the **stack**, pointing to the real object in memory.

| Stack            | Heap         
| :--------------- |:---------------:| 
| a = 5            |                 |
| b = a (5)        |                 |
| p1 (ref)-------->|[Object Person]  |
| p2 (ref)-------->|(Same Object) |

--- 

|Variable| Location | Value/Object
| :--------------- |:---------------:| -------
| a  | (Stack) | 5 |
| b  | (Stack) | 5 (copy of a)|
| p1 | Stack (Stack) | Reference to a Person object (Heap)|
| p2 | Stack | Reference to same Person object|
| Object | Person Tas (Heap) | Name = “Marie” (modified by p2)|

### 5. When is an object deleted by Garbage Collector?
Garbage Collector reclaims memory from unreferenced objects to prevent memory leaks.

Conditions for an object to be deleted:
- No more reference-type variables point to it.
- The object becomes inaccessible to code.
- The Garbage Collector decides to clean it up (this is not immediate, but done periodically).

## 2. Structs
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

## 3. Records
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

## 4. Exercices

### 1. Struct Basics:
- Create a Point struct with X and Y properties.
- Write a method to **calculate()** the distance between two points.
- Test value-type behavior by copying the struct and observing changes.

### 2. Immutable Struct:
- Create an immutable struct Rectangle with width and height.
- Add a method to calculate the area.
- Emphasize that struct fields should be readonly for immutability.

### 3. Record Basics:   
- Create a Person record with Name and Age.
- Demonstrate value-based equality by comparing two records with identical values.

### 4. Combining Structs and Records:
- Create a Shape record that has a Center (a Point struct) and a Radius.
- Show how structs (value types) and records (reference types) interact.

### 5. Performance Comparison:
- Create an exercise to compare performance between structs and classes in scenarios with large data sets (e.g., using arrays of structs versus classes).

### 6. Inheritance and Records:
- Create a base record Person and a derived record Employee.
- Show how records handle inheritance differently from classes (e.g., equality).


--- 

![](https://media3.giphy.com/media/v1.Y2lkPTc5MGI3NjExM2pjajhiZHF0OXRybHVuNGp5ODVjdTQ5Z2locTRvdmEyaWxsa2QyZSZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/Jam5tvnwAcBA4ZrbSO/giphy.gif)