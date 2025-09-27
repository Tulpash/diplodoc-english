# QuickSort

Выбираем опорный элемент (pivot), разделяем массив на элементы меньше и больше pivot, рекурсивно сортируем подмассивы. Эффективен в среднем случае, но может деградировать при плохом выборе pivot.
**Сложность:**
- Худший случай: O(n²) (при плохом pivot)
- Средний случай: O(n log n)
- Лучший случай: O(n log n)

```cs
using System;

class QuickSort
{
    // Метод разделения массива вокруг pivot
    static int Partition(int[] arr, int low, int high)
    {
        int pivot = arr[high]; // Опорный элемент (последний)
        int i = low - 1; // Индекс меньших элементов

        for (int j = low; j < high; j++) // Проходим по массиву
        {
            if (arr[j] < pivot)
            {
                i++; // Увеличиваем индекс
                // Обмен
                int temp = arr[i];
                arr[i] = arr[j];
                arr[j] = temp;
            }
        }
        // Помещаем pivot в правильное место
        int temp1 = arr[i + 1];
        arr[i + 1] = arr[high];
        arr[high] = temp1;
        return i + 1; // Возвращаем индекс pivot
    }

    // Рекурсивный метод быстрой сортировки
    static void QuickSortMethod(int[] arr, int low, int high)
    {
        if (low < high) // Базовый случай
        {
            int pi = Partition(arr, low, high); // Разделяем
            QuickSortMethod(arr, low, pi - 1); // Сортируем левую часть
            QuickSortMethod(arr, pi + 1, high); // Сортируем правую часть
        }
    }

    static void Main()
    {
        int[] arr = { 10, 7, 8, 9, 1, 5 }; // Пример массива
        QuickSortMethod(arr, 0, arr.Length - 1); // Вызов сортировки
        Console.WriteLine("Отсортированный массив: " + string.Join(" ", arr));
    }
}
```