# Финализаторы
Финализаторы (деструкторы) в C# используются редко, вызываются сборщиком мусора во время уборки мусора:
- При создании объекта, если у него есть деструктор то указатель на объект помещается в очередь финализации
- Во время уборки мусора если объект с финализатором недостижим то он перемещается в F-reachable очередь (очередб управляемая потоком финализации, который вызывает финализаторы объектов) - первая уборка мусора
- После выролнения финализатора объект доступен для удаления при слежующей уборке мусора (чаще всего второй, но это может быть достаточно большое число)

{% note warning %}

Таким образом объекты с финализатором требуют минимум две уборки мусора что создает дополнительную нагрузку на GC и увеличивает время жизни объекта

{% endnote %}

{% note warning %}

Две очереде ниобходимы для оптимизации работы GC:
- Они позволяют ассинхронно вызывать финализаторы (в очереди F-reachable, не блокируя основной поток)
- Очередь финализации поззволяет быстро найти объекты которым необходимо вызвать финализаторы
- Позволяет явно выделить состояние объекта - "Ожидает финализации" 

{% endnote %}

|Аспект|Очередь финализации|F-reachable|
|---|---|---|
|Назначение|Хранит ссылки на все объекты с финализаторами|Хранит ссылки на недостижимые объекты с финализаторами|
|Когда объект добавляется|При создании оюъекта если у нег оесть финализатор|При сборке мусора, если объект недостижим|
|Обработка|Сканируется GC дял проверки наличия объекта|Обрабатывается потоком финализации|
|Время жизни объекта|С момента создания до момента недостижимости|С момента недостижимости до финализации| 

{% note warning %}

Две очереде ниобходимы для оптимизации работы GC:
- Они позволяют ассинхронно вызывать финализаторы (в очереди F-reachable, не блокируя основной поток)
- Очередь финализации поззволяет быстро найти объекты которым необходимо вызвать финализаторы
- Позволяет явно выделить состояние объекта - "Ожидает финализации" 

{% endnote %}

{% note warning %}

Во время одной итерации сборки мусора GC вначале обходит граф объектов, а потом сканирует очередь финализации чтобы найти недостижимые объекты с финализаторами и поместить их в F-reachable очередь. На этом цикл сборки заканчивается. В фонофом потоке финализации проиходит обработка очереди F-reachable, для каждого объекта вызывается финализатор и указатель на объект удаляется из очереди, объект снвоа становится недостижимым. При последующих циклах сборки мусора GC вижит что этот объект не достижим и очищает память для него.

{% endnote %}