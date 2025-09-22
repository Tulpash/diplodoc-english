## C# 9 (.NET 5, ноябрь 2020)
{% cut "Records" %}
Это новый тип данных для неизменяемых (immutable) объектов с упрощённым синтаксисом. Они автоматически реализуют `equality`, `hash code` и `ToString()`. Подходят для DTO и данных телеметрии.
```cs
public record Person(string FirstName, string LastName);
var p1 = new Person("John", "Doe");
var p2 = new Person("John", "Doe");
Console.WriteLine(p1 == p2); // True (value-based equality)
```
{% endcut %}

{% cut "Init-only Properties" %}
Свойства с модификатором init можно устанавливать только при создании объекта, обеспечивая неизменяемость после инициализации.
```cs
public class Config
{
    public string Host { get; init; }
    public int Port { get; init; }
}
var config = new Config { Host = "example.com", Port = 8080 };
// config.Host = "new"; // Ошибка компиляции
```
{% endcut %}

{% cut "Top-level Statements" %}
Позволяет писать код без явного объявления `Main` метода, упрощая простые программы.
```cs
Console.WriteLine("Hello, World!");
```
{% endcut %}

{% cut "Pattern Matching Enhancements" %}
Улучшения в pattern matching: type patterns, relational patterns (`>`, `<`), logical patterns (`and`, `or`, `not`).
```cs
object value = 42;
if (value is int x and > 0)
{
    Console.WriteLine($"Positive integer: {x}");
}
```
{% endcut %}

{% cut "Covariant Returns" %}
Позволяет возвращать более конкретный тип в переопределённых методах.
```cs
abstract class Animal 
{ 
    public abstract Animal Clone(); 
}
class Dog : Animal 
{ 
    public override Dog Clone() => new Dog(); 
}
```
{% endcut %}

{% cut "Improved Target Typing" %}
Улучшения в выводе типов (например, `new()` без указания типа).
```cs
List<string> list = new();
```
{% endcut %}

{% cut "Fit and Finish" %}
Мелкие улучшения, такие как `with`-expressions для `records`, `new` для создания экземпляров без типа.
```cs
var p1 = new Person("John", "Doe");
var p2 = p1 with { LastName = "Smith" }; // Создаёт копию с изменённым LastName
```
{% endcut %}

## C# 10 (.NET 6, ноябрь 2021)
{% cut "Record Structs" %}
Records теперь доступны для структур, обеспечивая value-based equality для value types.
```cs
public record struct Point(int X, int Y);
var p1 = new Point(1, 2);
var p2 = new Point(1, 2);
Console.WriteLine(p1 == p2); // True
```
{% endcut %}

{% cut "Global Using Directives" %}
Позволяет объявлять using для всего проекта в одном файле.
```cs
global using System;
global using System.Linq;
```
{% endcut %}

{% cut "File-scoped Namespaces" %}
Упрощает объявление пространства имён без фигурных скобок.
```cs
namespace MyApp;
class Program { /* код */ }
```
{% endcut %}

{% cut "Improved Interpolated Strings" %}
Интерполированные строки могут быть `const`, если все части — константы.
```cs
const string name = "World";
const string greeting = $"Hello, {name}!"; // Компилируется как const
```
{% endcut %}

{% cut "Extended Property Patterns" %}
Улучшения в pattern matching для доступа к вложенным свойствам.
```cs
var obj = new { Person = new { Name = "John" } };
if (obj is { Person.Name: "John" })
    Console.WriteLine("Match");
```
{% endcut %}

{% cut "Lambda Improvements" %}
Лямбда-выражения могут указывать тип возвращаемого значения и атрибуты.
```cs
var add = [Pure] (int x, int y) => x + y;
```
{% endcut %}

{% cut "Sealed Record ToString" %}
Records позволяют переопределять ToString как sealed
```cs
public record Person(string Name) 
{ 
    public sealed override string ToString() => Name; 
}
```
{% endcut %}

## C# 11 (.NET 7, ноябрь 2022)
{% cut "Raw String Literals" %}
Многострочные строки с тройными кавычками ("""), игнорирующие escape-последовательности.
```cs
var json = """
    {
        "name": "John",
        "age": 30
    }
    """;
```
{% endcut %}

{% cut "Generic Attributes" %}
Атрибуты могут использовать generic-типы.
```cs
public class MyAttribute<T> : Attribute { }
[MyAttribute<string>]
class MyClass { }
```
{% endcut %}

{% cut "Required Members" %}
Свойства или поля с модификатором required требуют инициализации при создании объекта.
```cs
public class Person
{
    public required string Name { get; init; }
}
var p = new Person { Name = "John" }; // Name обязателен
```
{% endcut %}

{% cut "Generic Math" %}
Поддержка generic-интерфейсов (`INumber<T>`, `IAdditionOperators<T>`) для математических операций.
```cs
T Sum<T>(T[] values) where T : INumber<T>
{
    T sum = T.Zero;
    foreach (var value in values) sum += value;
    return sum;
}
```
{% endcut %}

{% cut "List Patterns" %}
Pattern matching для списков и массивов.
```cs
int[] numbers = { 1, 2, 3 };
if (numbers is [1, 2, 3]) 
    Console.WriteLine("Match");
```
{% endcut %}

{% cut "Auto-default Structs" %}
Структуры автоматически инициализируют неуказанные поля значением по умолчанию.
```cs
struct Point { public int X; public int Y; }
Point p = new() { X = 1 }; // Y = 0 автоматически
```
{% endcut %}

{% cut "Improved Method Group Conversion" %}
Упрощённое преобразование делегатов.
```cs
Action<int> action = Console.WriteLine; // Прямая конверсия
```
{% endcut %}

## C# 12 (.NET 8, ноябрь 2023)
{% cut "Primary Constructors" %}
Конструкторы в определении класса/структуры, упрощающие синтаксис.
```cs
public class Person(string name, int age)
{
    public string Name => name;
    public int Age => age;
}
```
{% endcut %}

{% cut "Collection Expressions" %}
Упрощённый синтаксис для создания коллекций.
```cs
int[] array = [1, 2, 3];
List<int> list = [4, 5, 6];
```
{% endcut %}

{% cut "Inline Arrays" %}
Фиксированные массивы в `struct` для оптимизации производительности.
```cs
[System.Runtime.CompilerServices.InlineArray(10)]
public struct Buffer
{
    private int _element0;
}
var buffer = new Buffer();
buffer[0] = 1; // Как массив
```
{% endcut %}

{% cut "Alias Any Type" %}
`using` для любых типов, включая кортежи и примитивы.
```cs
using Point = (int X, int Y);
Point p = (1, 2);
```
{% endcut %}

{% cut "Default Lambda Parameters" %}
Лямбда-выражения с параметрами по умолчанию.
```cs
var greet = (string name = "World") => $"Hello, {name}!";
```
{% endcut %}

{% cut "Ref Readonly Parameters" %}
`ref readonly` для передачи по ссылке без модификации.
```cs
void Process(ref readonly int value) 
{ 
    Console.WriteLine(value); 
}
```
{% endcut %}

## C# 13 (.NET 9, ноябрь 2024)
{% cut "Implicit Index Access" %}
Упрощённый доступ к индексам без явного создания `Index`.
```cs
int[] array = [0, 1, 2, 3];
var last = array[^1]; // Не требует `Index` явным образом
```
{% endcut %}

{% cut "Extension params" %}
`params` теперь поддерживает пользовательские типы коллекций.
```cs
void Log<T>(params ReadOnlySpan<T> items)
{
    foreach (var item in items) Console.WriteLine(item);
}
Log(1, 2, 3); // Работает с Span<T>
```
{% endcut %}

{% cut "Lock Object Improvements" %}
Улучшения в `lock`, включая поддержку любых объектов с `Monitor`.
```cs
var obj = new object();
lock (obj) { /* критическая секция */ }
```
{% endcut %}

{% cut "Scoped Ref Structs" %}
Улучшения в анализе escape-проблем для ref structs (`Span<T>`).
```cs
Span<int> Process(Span<int> data)
{
    return data[1..3]; // Более строгий анализ lifetime
}
```
{% endcut %}

{% cut "Field Keyword in Lambda Expressions" %}
Доступ к полям через лямбда-выражения.
```cs
record Person(string Name);
var getName = (Person p) => p.field.Name;
```
{% endcut %}

{% cut "Semi-auto Properties" %}
Свойства с автоматическим backing field, но с возможностью кастомной логики.
```cs
public class Person
{
    public string Name { get => field; init => field = value ?? throw new ArgumentNullException(); }
}
```
{% endcut %}