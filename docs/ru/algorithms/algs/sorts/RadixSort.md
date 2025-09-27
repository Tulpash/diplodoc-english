# RadixSort

Не сравнительный алгоритм, сортирующий элементы по отдельным разрядам (цифрам) от младшего к старшему (LSD) или наоборот (MSD). Для каждого разряда используется стабильная сортировка (часто Counting Sort). Подходит для сортировки целых чисел, строк или ключей с фиксированной длиной. Эффективен, когда количество разрядов мало по сравнению с n.

**Сложность:**
- Худший случай: O(d * (n + k)), где d — количество разрядов, k — диапазон значений (для цифр k=10)
- Средний случай: O(d * (n + k))
- Лучший случай: O(d * (n + k))

```cs
using System;

class RadixSort
{
    // Вспомогательный метод: Counting Sort по разряду
    static void CountSort(int[] arr, int exp)
    {
        int n = arr.Length;
        int[] output = new int[n]; // Выходной массив
        int[] count = new int[10]; // Счетчик для 0-9

        // Считаем частоты
        for (int i = 0; i < n; i++)
            count[(arr[i] / exp) % 10]++;

        // Кумулятивные суммы
        for (int i = 1; i < 10; i++)
            count[i] += count[i - 1];

        // Строим output
        for (int i = n - 1; i >= 0; i--)
        {
            output[count[(arr[i] / exp) % 10] - 1] = arr[i];
            count[(arr[i] / exp) % 10]--;
        }

        // Копируем в arr
        Array.Copy(output, arr, n);
    }

    // Основной метод Radix Sort
    static void RadixSortMethod(int[] arr)
    {
        int n = arr.Length;
        if (n == 0) return;

        // Находим максимум для количества разрядов
        int max = arr[0];
        for (int i = 1; i < n; i++)
            if (arr[i] > max) max = arr[i];

        // Сортируем по каждому разряду
        for (int exp = 1; max / exp > 0; exp *= 10)
            CountSort(arr, exp);
    }

    static void Main()
    {
        int[] arr = { 170, 45, 75, 90, 802, 24, 2, 66 }; // Пример массива
        RadixSortMethod(arr); // Вызов сортировки
        Console.WriteLine("Отсортированный массив: " + string.Join(" ", arr));
    }
}
```