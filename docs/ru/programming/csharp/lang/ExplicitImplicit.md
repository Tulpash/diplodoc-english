# Explicit
Оператор явного преобразования.
```cs
public class Distance
{
    private double meters;

    public Distance(double meters) => this.meters = meters;

    // Явное преобразование из double в Distance
    public static explicit operator Distance(double meters) => new Distance(meters);
}

class Program
{
    static void Main()
    {
        double meters = 5.0;
        Distance d = (Distance)meters; // Требуется явное приведение
        Console.WriteLine(d); // Вывод зависит от ToString
    }
}
```

# Implicit
Оператор неявного преобразования.
```cs
public class Distance
{
    private double meters;

    public Distance(double meters) => this.meters = meters;

    // Неявное преобразование из Distance в double
    public static implicit operator double(Distance distance) => distance.meters;
}

class Program
{
    static void Main()
    {
        Distance d = new Distance(5.0);
        double meters = d; // Неявное преобразование, без (double)
        Console.WriteLine(meters); // Вывод: 5
    }
}
```