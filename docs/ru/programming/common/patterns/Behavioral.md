# Поведенчиские паттерны

## Observer
**Идея:** Определяет зависимость "один ко многим", где изменение одного объекта уведомляет всех зависимых.
**Назначение:** Используется для событийной модели (например, в UI или pub/sub-системах).
**Внутри:** Субъект хранит список наблюдателей и уведомляет их при изменении состояния.
**Когда использовать:** Для событий, подписок (например, event в C#).
```cs
public interface IObserver { void Update(string message); }
public class Subject
{
    private List<IObserver> observers = new List<IObserver>();
    public void AddObserver(IObserver o) => observers.Add(o);
    public void Notify(string message) => observers.ForEach(o => o.Update(message));
}
```

## Strategy
**Идея:** Определяет семейство алгоритмов, инкапсулируя их и позволяя подменять во время выполнения.
**Назначение:** Позволяет выбирать алгоритм в зависимости от контекста.
**Внутри:** Интерфейс для алгоритмов, которые реализуются в отдельных классах.
**Когда использовать:** Для выбора алгоритмов (например, сортировка, компрессия).
```cs
public interface IStrategy { int Execute(int a, int b); }
public class AddStrategy : IStrategy { public int Execute(int a, int b) => a + b; }
public class Context
{
    private IStrategy strategy;
    public Context(IStrategy s) => strategy = s;
    public int ExecuteStrategy(int a, int b) => strategy.Execute(a, b);
}
```

## Command
**Идея:** Инкапсулирует запрос как объект, позволяя передавать его, ставить в очередь или отменять.
**Назначение:** Удобен для реализации undo/redo, очередей задач.
**Внутри:** Команда содержит метод выполнения и, возможно, отмены.
**Когда использовать:** Для операций с возможностью отмены (например, редакторы).
```cs
public interface ICommand { void Execute(); }
public class ConcreteCommand : ICommand
{
    private Receiver receiver;
    public ConcreteCommand(Receiver r) => receiver = r;
    public void Execute() => receiver.Action();
}
```

## Template Method
**Идея:** Определяет скелет алгоритма, позволяя подклассам переопределять отдельные шаги.
**Назначение:** Устанавливает общий порядок действий, но даёт гибкость в деталях.
**Внутри:** Абстрактный класс с шаблонным методом, где некоторые шаги переопределяются.
**Когда использовать:** Для алгоритмов с фиксированной структурой (например, парсинг файлов).
```cs
public abstract class Template
{
    public void TemplateMethod()
    {
        Step1();
        Step2();
    }
    protected abstract void Step1();
    protected abstract void Step2();
}
```