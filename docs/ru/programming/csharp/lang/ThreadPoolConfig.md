## ThreadPool конфигурация

## MinThreads
Задаёт минимальное число рабочих и I/O-потоков в пуле.
- Код: `ThreadPool.SetMinThreads(workerThreads, completionPortThreads)`
- Environment: `DOTNET_ThreadPoolMinThreads=50` (неофициально, зависит от версии)
- .NET Framework: `<threadPool minWorkerThreads="50" minIOThreads="50"/>`
**Пример использования:** Высоконагруженные серверы, где много задач сразу.

## MaxThreads
Задаёт максимальное число рабочих и I/O-потоков.
- Код: `ThreadPool.SetMaxThreads(workerThreads, completionPortThreads)`
- Environment: `DOTNET_ThreadPoolMaxThreads=1000` (неофициально, зависит от версии)
- .NET Framework: `<threadPool maxWorkerThreads="1000" maxIOThreads="1000"/>`
**Пример использования:** Ограничить потребление ресурсов в контейнерах.

## Thread Priority
Приоритет потоков ThreadPool (по умолчанию `Normal`).
Только через создание нового Thread с Thread.Priority, так как ThreadPool не позволяет менять приоритет пула.
**Пример использования:** Редко, так как ThreadPool оптимизирован для равномерной нагрузки.