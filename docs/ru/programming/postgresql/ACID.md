## ACID в PostgreSQL
Здесь описаны прицнипы **ACID** в рамках **PostgreSQL**.

# Atomicity
- PostgreSQL использует **MVCC (Multiversion Concurrency Control)** для управления транзакциями. Каждая транзакция работает со снимком данных, с помощью этого достигается изоляция.
- Все изменения при выполнении транзакции предварительно хаписываются в WAL (Write-Ahead Log) - журнал упреждающей записи. Это временный лог, который хранит все операции жо их фиксации.
- Если транзакция  завершится успешно (`COMMIT`), то изменения фиксируются в БД. Если происходит ошибка (`ROLLBACK`), изменения из **WAL** не применяются у данным.
- PostgreSQL использует **Transaction ID (XID)** для отслеживания транзакций. Если транзакция прерывается из-за сбоя, то незавершенные операции откатываются при перезапуске, используя **WAL** и **XID**.

# Consistensy
- PostgreSQL проверяет все ограничения целостности (`PK`, `FK`, `CHECK`, `UNIQUE`, ...) в момент выполнения транзакции.
- Если ограничения нарушено, то транзакция завершиться ошибкой и изменения откатятся.
- Триггеры и хранимые процедуры помогают поддерживать целостность выплолняя пользовательскую логику.
- В PostgreSQL есть дефферируемые ограничения (deffered constraints), которые можно проверять не сразу а в конце транзакции.

# Isolation
- Механизм MVCC отвечатает за то что каждая транзакция работает с отдельным снимком данных, это предотвращает **Dirty read**.
- PostgreSQL использует блокировки на уровне таблиц и строк.

# Durabillity
- PostgreSQL использует **WAL** для обеспечения надежности.
- При завершении коммита вызывается `fsync`, что ы гарантировать что данные о транзакции из **WAL** запишутся на диск, только после этого коммит считается полностью завершенным.
- Для повышения производительности модно использовать параметр `synchronous_commit`:
    - `on`: (По умолчанию) коммит завершится полностью только после записи данных при помощи `fsync`.
    - `off`: Коммит может завершиться до записи на диск при помощи `fsync`.
- PostgreSQL поддержитвает `chekpoint` внутри транзакции для переолической синхронизации данных из WAL с таблицами, тем самым минимизируя объем данных, которые нужн овосстановить после сбоя.