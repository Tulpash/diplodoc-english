# Перехватчики (Interceptors)
Перехватчики это классы позводяющие перехватывать определенные события в жизненном цикле контекста:
- Выполнение SQL-запросов (**ICommandInterceptor**)
- Сохранение изменений в БД (**ISaveChangesInterceptor**)
- Управленеи транзакциями (**ITransactionInterceptor**)
- События соединения (**IConnectionInterceptor**)

Перехватчики регистрируются в *DbContext* и вызываются в нужный момент сами, абстрактная концепция похожа на iddleware.

## ICommandInterceptor
Используется для перехвата запросов к БД, например: `SELECT`, `INSERT`, `UPDATE`, `DELETE` и т.д.
Методы:
- ScalarExecuting/ScalarExecuted
- ReaderExecuting/ReaderExecuted
- NonQueryExecuting/NonQueryExecuted

## ISaveChangesInterceptor
Ловит вызовы SaveChanges/SaveChangesAsync.
Методы:
- SavingChanges/SavingChangesAsync
- SavedChanges/SavedChangesAsync
- SaveChangesFailed/SaveChangesFailedAsync

## IConnectionInterceptor
Перезватывает события связанные с соединением.
Методы:
- ConnectionOpening/ConnectionOpened
- ConnectionClosing/ConnectionClosed

## ITransactionInterceptor
Перехватывает события транзакций.
Методы:
- TransactionStarted
- TransactionCommited
- TransactionRolledBack

## Пример
Например мы хотим логировать все запросы и время их выполнения.
```cs
public class LoggingCommandInterceptor : DbCommandInterceptor
{
    private readonly Stopwatch _stopwatch = new Stopwatch();
    private readonly Guid _id = Guid.NewGuid();

    public override DbDataReader ReaderExecuting(
        DbCommand command,
        CommandEventData eventData,
        DbDataReader result)
    {
        _stopwatch.Restart();
        Console.WriteLine($"{_id} Executing SQL: {command.CommandText}");
        return base.ReaderExecuting(command, eventData, result);
    }

    public override void ReaderExecuted(
        DbCommand command,
        CommandEventData eventData,
        DbDataReader result)
    {
        _stopwatch.Stop();
        Console.WriteLine($"{_id} SQL executed in {_stopwatch.ElapsedMilliseconds} ms");
        base.ReaderExecuted(command, eventData, result);
    }
}
```

{% note info %}

[DbCommandInterceptor](https://learn.microsoft.com/en-us/dotnet/api/microsoft.entityframeworkcore.diagnostics.dbcommandinterceptor?view=efcore-9.0) наследует **ICommandInterceptor**

{% endnote %}

Потом надо зарегистрировать перехватчик в DI.
```cs
services.AddDbContext<MyDbContext>(options =>
    options.UseSqlServer(connectionString)
           .AddInterceptors(new LoggingCommandInterceptor()));
```