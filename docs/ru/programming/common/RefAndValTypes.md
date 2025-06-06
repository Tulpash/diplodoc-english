# Ссылочные и значимые типы

## Что такое значимые и ссылочные типы?
**Значимые типы (Value types)**:
- Хранят свои значения непосредственно там где были объявлены (обычно на стеке)
- При присваивании или передачи в функцию создается копия
- Имеют фиксированный размер в памяти
- Имеют высокую производительность из-за отсутствия накладных расходов на управление кучей

{% note info "Примеры" %}

int, float, bool, struct, enum

{% endnote %}

**Ссылочные типы (Reference types):**
- Хранят ссылку (указатель) на данные в куче
- Сама ссылка имеет фиксирвоанный размер и лежит на стеке
- При присваивании или передаче в функцию передается ссылка а не копия данных
- Более гибкие, гл менее производительные из-за необходимости управления кучей кучи

{% note info "Примеры" %}

class, array, string, interface, pointer

{% endnote %}

## Как устроены в памяти?
**Значимые типы (Value types)**:
Хранятся в [стеке](*stack_key), в котормо выделяется память фиксированного размера

**Ссылочные типы (Reference types):**
Хранятся в [куче](*heap_key), а в стеке хранится только ссылка(указатель) на данные в куче

[*stack_key]: Это быстродоступная область памяти, которая работает по принципу LIFO (Last In, First Out).
[*heap_key]: Это область памяти, управляемая сборщиком мусора (Garbage Collector, GC) в C# и Go.

## Сравнение для C# и Go
|Аспект|C#|Go|
|---|---|---|
|Значимые типы|`int`, `struct`, `enum` и т.д. Хранятся в стеке или куче (если част часть класса).|`int`, `struct`, массивы, строки. Хранятся в стеке или куче (escape analysis).|
|Ссылочные типы|`class`, `string`, массивы, интерфейсы. Хранятся в куче.|Указатели `*T`, срезы, мапы, каналы. Хранятся в куче.|
|Копирование|Значимые: копия значения. Ссылочные: копия ссылки.|Значимые: копия значения. Ссылочные: копия ссылки|
|Управление памятью|Сборка мусора (.NET GC). Boxing/unboxing для занчимых типов.|Сборка мусора. Escape analysis для аллокации в куче.|
|Классы vs Структуры|`class`(ссылочный), `struct`(значимый)|Только `struct`. Ссылочность через указатели.|
|Передача по ссылке|`ref`, `out`, `in` для значимыз типов.|Указатели `*T` для передачи по ссылке.|
|Производительность|Boxing/unboxing очень затратен. Структкры быстрее дял малых данных.|Осторожность с указателями для избегания лишних аллокаций.|