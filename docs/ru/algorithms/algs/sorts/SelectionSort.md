# Сортировка выбором
На каждом шаге находим минимальный элемент в неотсортированной части массива и меняем его с первым элементом этой части. Отсортированная часть растет слева.

**Сложность:**
- Худший случай: O(n²)
- Средний случай: O(n²)
- Лучший случай: O(n²)

```cs
using System;

class SelectionSort
{
    // Метод для сортировки выбором
    static void SelectionSortMethod(int[] arr)
    {
        int n = arr.Length; // Длина массива
        for (int i = 0; i < n - 1; i++) // Цикл по неотсортированной части
        {
            int minIdx = i; // Индекс минимального элемента
            for (int j = i + 1; j < n; j++) // Поиск минимума
            {
                if (arr[j] < arr[minIdx])
                {
                    minIdx = j; // Обновляем индекс минимума
                }
            }
            // Обмен с первым элементом неотсортированной части
            int temp = arr[minIdx];
            arr[minIdx] = arr[i];
            arr[i] = temp;
        }
    }

    static void Main()
    {
        int[] arr = { 64, 25, 12, 22, 11 }; // Пример массива
        SelectionSortMethod(arr); // Вызов сортировки
        Console.WriteLine("Отсортированный массив: " + string.Join(" ", arr));
    }
}
```