# Пирамидальная сортировка

Строим max-heap (кучу), где родитель больше детей. Затем извлекаем максимумы, перестраивая кучу. Это эффективный алгоритм без рекурсии.

**Сложность:**
- Худший случай: O(n log n)
- Средний случай: O(n log n)
- Лучший случай: O(n log n)

```cs
using System;

class HeapSort
{
    // Метод для перестройки кучи
    static void Heapify(int[] arr, int n, int i)
    {
        int largest = i; // Корень
        int l = 2 * i + 1; // Левый ребенок
        int r = 2 * i + 2; // Правый ребенок

        // Проверяем левый ребенок
        if (l < n && arr[l] > arr[largest])
            largest = l;

        // Проверяем правый ребенок
        if (r < n && arr[r] > arr[largest])
            largest = r;

        // Если largest не корень, меняем и продолжаем
        if (largest != i)
        {
            int swap = arr[i];
            arr[i] = arr[largest];
            arr[largest] = swap;
            Heapify(arr, n, largest); // Рекурсия
        }
    }

    // Метод пирамидальной сортировки
    static void HeapSortMethod(int[] arr)
    {
        int n = arr.Length; // Длина массива

        // Строим max-heap
        for (int i = n / 2 - 1; i >= 0; i--)
            Heapify(arr, n, i);

        // Извлекаем элементы
        for (int i = n - 1; i > 0; i--)
        {
            // Меняем корень с последним
            int temp = arr[0];
            arr[0] = arr[i];
            arr[i] = temp;

            // Перестраиваем кучу
            Heapify(arr, i, 0);
        }
    }

    static void Main()
    {
        int[] arr = { 12, 11, 13, 5, 6, 7 }; // Пример массива
        HeapSortMethod(arr); // Вызов сортировки
        Console.WriteLine("Отсортированный массив: " + string.Join(" ", arr));
    }
}
```