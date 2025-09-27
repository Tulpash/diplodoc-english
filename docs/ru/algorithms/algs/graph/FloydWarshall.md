# Алгоритм Флойда-Уоршелла

Это классический алгоритм динамического программирования, используемый для поиска кратчайших путей между всеми парами вершин в взвешенном графе. Он был разработан Робертом Флойдом в 1962 году на основе работ Бернарда Роя и Стивена Уоршелла. Этот алгоритм особенно полезен для плотных графов и может обрабатывать графы с отрицательными весами рёбер (но не с отрицательными циклами, так как они делают задачу неопределённой).

```cs
using System;

class FloydWarshall
{
    private const int INF = int.MaxValue / 2; // Чтобы избежать переполнения

    public void FindShortestPaths(int[,] graph, int vertices)
    {
        int[,] dist = new int[vertices, vertices];

        // Инициализация матрицы
        for (int i = 0; i < vertices; i++)
        {
            for (int j = 0; j < vertices; j++)
            {
                dist[i, j] = graph[i, j];
            }
        }

        // Основной цикл Флойда-Уоршелла
        for (int k = 0; k < vertices; k++)
        {
            for (int i = 0; i < vertices; i++)
            {
                for (int j = 0; j < vertices; j++)
                {
                    if (dist[i, k] != INF && dist[k, j] != INF &&
                        dist[i, k] + dist[k, j] < dist[i, j])
                    {
                        dist[i, j] = dist[i, k] + dist[k, j];
                    }
                }
            }
        }

        // Вывод результатов
        Console.WriteLine("Матрица кратчайших расстояний:");
        for (int i = 0; i < vertices; i++)
        {
            for (int j = 0; j < vertices; j++)
            {
                if (dist[i, j] == INF)
                    Console.Write("∞ ");
                else
                    Console.Write(dist[i, j] + " ");
            }
            Console.WriteLine();
        }

        // Проверка на отрицательные циклы
        for (int i = 0; i < vertices; i++)
        {
            if (dist[i, i] < 0)
            {
                Console.WriteLine("Граф содержит отрицательный цикл!");
                return;
            }
        }
    }
}

class Program
{
    static void Main()
    {
        int vertices = 4;
        int[,] graph = new int[vertices, vertices];

        // Инициализация графа (∞ = INF)
        for (int i = 0; i < vertices; i++)
            for (int j = 0; j < vertices; j++)
                graph[i, j] = (i == j) ? 0 : FloydWarshall.INF;

        // Добавление рёбер
        graph[0, 1] = 3;
        graph[0, 2] = 5;
        graph[1, 2] = 2;
        graph[1, 3] = 6;
        graph[2, 3] = 1;

        // Для отрицательного ребра (пример): graph[3, 1] = -2; (но без цикла)

        FloydWarshall fw = new FloydWarshall();
        fw.FindShortestPaths(graph, vertices);
    }
}
```