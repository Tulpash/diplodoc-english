# TimSort

TimSort — это гибридный алгоритм, сочетающий сортировку слиянием (Merge Sort) и сортировку вставками (Insertion Sort). Он делит массив на "runs" (естественно отсортированные подпоследовательности), сортирует маленькие runs вставками, а затем сливает их с помощью модифицированного Merge Sort. Это делает его стабильным и очень эффективным на реальных данных, где часто встречаются частично отсортированные последовательности. TimSort используется в Python (sorted()), Java (Arrays.sort()) и .NET (Array.Sort для примитивов).

**Сложность:**
- Худший случай: O(n log n)
- Средний случай: O(n log n)
- Лучший случай: O(n) (если массив уже отсортирован)

```cs
using System;

class TimSort
{
    const int MIN_MERGE = 32; // Минимальный размер run для слияния

    // Метод для сортировки вставками маленьких подмассивов
    static void InsertionSort(int[] arr, int left, int right)
    {
        for (int i = left + 1; i <= right; i++)
        {
            int temp = arr[i];
            int j = i - 1;
            while (j >= left && arr[j] > temp)
            {
                arr[j + 1] = arr[j];
                j--;
            }
            arr[j + 1] = temp;
        }
    }

    // Метод слияния двух runs
    static void Merge(int[] arr, int l, int m, int r)
    {
        int len1 = m - l + 1, len2 = r - m;
        int[] left = new int[len1];
        int[] right = new int[len2];
        Array.Copy(arr, l, left, 0, len1);
        Array.Copy(arr, m + 1, right, 0, len2);

        int i = 0, j = 0, k = l;
        while (i < len1 && j < len2)
        {
            if (left[i] <= right[j])
                arr[k++] = left[i++];
            else
                arr[k++] = right[j++];
        }
        while (i < len1) arr[k++] = left[i++];
        while (j < len2) arr[k++] = right[j++];
    }

    // Основной метод TimSort
    static void TimSortMethod(int[] arr)
    {
        int n = arr.Length;
        if (n < 2) return;

        // Находим и сортируем runs
        for (int i = 0; i < n; i += MIN_MERGE)
        {
            InsertionSort(arr, i, Math.Min(i + MIN_MERGE - 1, n - 1));
        }

        // Сливаем runs, удваивая размер
        for (int size = MIN_MERGE; size < n; size = 2 * size)
        {
            for (int left = 0; left < n; left += 2 * size)
            {
                int mid = left + size - 1;
                int right = Math.Min(left + 2 * size - 1, n - 1);
                if (mid < right)
                    Merge(arr, left, mid, right);
            }
        }
    }

    static void Main()
    {
        int[] arr = { 5, 21, 7, 23, 19, 10, 15, 3 }; // Пример массива
        TimSortMethod(arr); // Вызов сортировки
        Console.WriteLine("Отсортированный массив: " + string.Join(" ", arr));
    }
}
```