# Task.Yield()

Метод `Task.Yield` при вызова планирует продолжение ка котдельную задачу и кладет ее в очередь `TaskScheduler`-а. Таким образом фоновая задача которая выполнялась бы в одном потоке разбивается на несколько задач по меньше что бы не монополизировать поток или ресурсы которые она использует. По сути задача просто "рвется" на кусочки что бы другие задачи так же могли выполнятся.

## Без Task.Yield:
```cs
Console.WriteLine($"Главный поток: {Thread.CurrentThread.ManagedThreadId}");
Task t = Task.Run(() =>
{
    for (int i = 0; i < 3; i++)
    {
        Console.WriteLine($"Task 1: {i}, до Yield, поток: {Thread.CurrentThread.ManagedThreadId}");
        Console.WriteLine($"Task 1: {i}, после Yield, поток: {Thread.CurrentThread.ManagedThreadId}");
    }
});
await Task.Run(() => 
{
    for (int i = 0; i < 3; i++)
    {
        Console.WriteLine($"Task 2: {i}, до Yield, поток: {Thread.CurrentThread.ManagedThreadId}");
        Console.WriteLine($"Task 2: {i}, после Yield, поток: {Thread.CurrentThread.ManagedThreadId}");
    }
});
await t;
Console.WriteLine("Завершено");
```
**Вывод:**
```
Главный поток: 1
Task 1: 0, до Yield, поток: 5
Task 1: 0, после Yield, поток: 5
Task 1: 1, до Yield, поток: 5
Task 2: 0, до Yield, поток: 7
Task 1: 1, после Yield, поток: 5
Task 1: 2, до Yield, поток: 5
Task 1: 2, после Yield, поток: 5
Task 2: 0, после Yield, поток: 7
Task 2: 1, до Yield, поток: 7
Task 2: 1, после Yield, поток: 7
Task 2: 2, до Yield, поток: 7
Task 2: 2, после Yield, поток: 7
Завершено
```
Видно что какждая задача выполняется непрерывно в одном потоке.

## С Task.Yield:
```cs
Console.WriteLine($"Главный поток: {Thread.CurrentThread.ManagedThreadId}");
Task t = Task.Run(async () =>
{
    for (int i = 0; i < 3; i++)
    {
        Console.WriteLine($"Task 1: {i}, до Yield, поток: {Thread.CurrentThread.ManagedThreadId}");
        await Task.Yield();
        Console.WriteLine($"Task 1: {i}, после Yield, поток: {Thread.CurrentThread.ManagedThreadId}");
    }
});
await Task.Run(async () => 
{
    for (int i = 0; i < 3; i++)
    {
        Console.WriteLine($"Task 2: {i}, до Yield, поток: {Thread.CurrentThread.ManagedThreadId}");
        await Task.Yield();
        Console.WriteLine($"Task 2: {i}, после Yield, поток: {Thread.CurrentThread.ManagedThreadId}");
    }
});
await t;
Console.WriteLine("Завершено");
```
**Вывод:**
```
Главный поток: 1
Task 1: 0, до Yield, поток: 5
Task 2: 0, до Yield, поток: 7
Task 2: 0, после Yield, поток: 8
Task 2: 1, до Yield, поток: 8
Task 1: 0, после Yield, поток: 7
Task 1: 1, до Yield, поток: 7
Task 2: 1, после Yield, поток: 5
Task 2: 2, до Yield, поток: 5
Task 1: 1, после Yield, поток: 7
Task 1: 2, до Yield, поток: 7
Task 2: 2, после Yield, поток: 5
Task 1: 2, после Yield, поток: 7
Завершено
```
Видно что каждая задача использует несколько потоков.

## Зачем это нужно
Что бы задачи не приватизировалм потоки, если запустить много долгих задач то они займут все потоки и другие задачи будут ждать пока не завершитсья какая-нибудь долгая задача.