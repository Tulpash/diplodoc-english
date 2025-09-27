# Сортировка подсчетом

Не сравнительный алгоритм для сортировки целых чисел в ограниченном диапазоне. Создается массив счетчиков для каждого возможного значения, затем на основе кумулятивных сумм строится отсортированный массив. Стабильный, часто используется как подпрограмма в Radix Sort. Актуален для данных с малым диапазоном (например, оценки от 0 до 100).

**Сложность:**
- Худший случай: O(n + k), где k — диапазон значений
- Средний случай: O(n + k)
- Лучший случай: O(n + k)

```cs
using System;

class CountingSort
{
    // Метод Counting Sort
    static void CountingSortMethod(int[] arr, int maxValue)
    {
        int n = arr.Length;
        int[] output = new int[n]; // Выходной массив
        int[] count = new int[maxValue + 1]; // Счетчики

        // Считаем частоты
        for (int i = 0; i < n; i++)
            count[arr[i]]++;

        // Кумулятивные суммы
        for (int i = 1; i <= maxValue; i++)
            count[i] += count[i - 1];

        // Строим output (с конца для стабильности)
        for (int i = n - 1; i >= 0; i--)
        {
            output[count[arr[i]] - 1] = arr[i];
            count[arr[i]]--;
        }

        // Копируем в arr
        Array.Copy(output, arr, n);
    }

    static void Main()
    {
        int[] arr = { 4, 2, 2, 8, 3, 3, 1 }; // Пример массива
        int maxValue = 8; // Максимальное значение в массиве
        CountingSortMethod(arr, maxValue); // Вызов сортировки
        Console.WriteLine("Отсортированный массив: " + string.Join(" ", arr));
    }
}
```