# Структурные паттерны

## Adapter
**Идея:** Преобразует интерфейс одного класса в интерфейс, ожидаемый клиентом.
**Назначение:** Позволяет несовместимым классам работать вместе.
**Внутри:** Адаптер реализует целевой интерфейс и оборачивает адаптируемый объект, транслируя вызовы.
**Когда использовать:** Интеграция со сторонними библиотеками или устаревшими системами.
```cs
public interface ITarget { void Request(); }
public class Adaptee { public void SpecificRequest() { } }
public class Adapter : ITarget
{
    private readonly Adaptee adaptee = new Adaptee();
    public void Request() => adaptee.SpecificRequest();
}
```

## Decorator
**Идея:** Динамически добавляет обязанности объекту, оборачивая его.
**Назначение:** Расширяет функциональность без изменения исходного класса.
**Внутри:** Класс-обёртка реализует тот же интерфейс и делегирует вызовы, добавляя свою логику.
**Когда использовать:** Для добавления логирования, кэширования или других аспектов.
```cs
public interface IComponent { string Operation(); }
public class ConcreteComponent : IComponent { public string Operation() => "Base"; }
public class Decorator : IComponent
{
    private readonly IComponent component;
    public Decorator(IComponent c) => component = c;
    public string Operation() => $"Decorated({component.Operation()})";
}
```

## Facade
**Идея:** Предоставляет упрощённый интерфейс к сложной подсистеме.
**Назначение:** Скрывает сложность подсистемы, упрощая её использование.
**Внутри:** Класс-фасад инкапсулирует взаимодействие с несколькими классами подсистемы.
**Когда использовать:** Для упрощения API сложных библиотек.
```cs
public class Facade
{
    private readonly SubsystemA a = new SubsystemA();
    private readonly SubsystemB b = new SubsystemB();
    public string Operation() => a.OperationA() + b.OperationB();
}
```

## Composite
**Идея:** Позволяет работать с группами объектов как с единым объектом, представляя иерархию.
**Назначение:** Удобен для древовидных структур (например, UI-компоненты, файловые системы).
**Внутри:** Общий интерфейс для листьев и контейнеров, которые могут содержать другие элементы.
**Когда использовать:** Для иерархий, таких как DOM или графические элементы.
```cs
public interface IComponent { void Operation(); }
public class Leaf : IComponent { public void Operation() { } }
public class Composite : IComponent
{
    private List<IComponent> children = new List<IComponent>();
    public void Add(IComponent c) => children.Add(c);
    public void Operation() => children.ForEach(c => c.Operation());
}
```