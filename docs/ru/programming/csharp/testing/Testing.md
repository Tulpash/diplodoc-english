# Тестирование
Для написания тестов на C# использются библиоткеи: NUnit, xUnit, Moq.

## Moq
Библиотека для написания моковых реализаций зависимостей.
{% end note "Установка" %}

dotnet add package Moq

{% endnote %}

Основные концепции:
- Mock<T>: Основной класс для создания моков объекта T
- Setup: Метод для настройки поведения моков
- Verify: Метод для проверки, был ли вызван метож с определнным параметром
- Returns: Указывает что возвращать при вызове метода
- Throws: Указывает какое исключение должен вызвать метод

Примеры:
{% cut "" %}
```cs
public interface IUserRepository
{
    Task<User> GetUserByIdAsync(int id);
}

IUserRepository mockRepo = new Mock<IUserRepository>();
        mockRepo.Setup(repo => repo.GetUserByIdAsync(1))
                .ReturnsAsync(new User { Id = 1, Name = "Alice" });
```

{% endcut %}

{% cut "Мокирование свойств" %}

```cs
mockRepo.SetupProperty(repo => repo.SomeProperty, "value");
```

{% endcut %}

{% cut "Мокирование всех методов интерфейса" %}

```cs
var mock = new Mock<IUserRepository>(MockBehavior.Loose); // Методы возвращают значения по умолчанию
```

{% endcut %}

{% cut "Callback для записи параметров" %}

```cs
mockRepo
    .Setup(repo => repo.SaveUser(It.IsAny<User>()))
    .Callback<User>(user => Console.WriteLine(user.Name));
```

{% endcut %}

{% cut "Мокирование исключений" %}

```cs
mockRepo
    .Setup(repo => repo.GetUserByIdAsync(1))
    .ThrowsAsync(new InvalidOperationException("Database error"));
```

{% endcut %}

{% cut "Мокирование последовательных вызовов" %}

```cs
mockRepo
    .SetupSequence(repo => repo.GetUserByIdAsync(1))
    .ReturnsAsync(new User { Id = 1, Name = "Alice" })
    .ReturnsAsync(new User { Id = 1, Name = "Bob" });
```

{% endcut %}

{% end note "Альтернативы" %}

- NSubstitute
- FakeItEasy
- Ручные моки

{% endnote %}

## xUnit