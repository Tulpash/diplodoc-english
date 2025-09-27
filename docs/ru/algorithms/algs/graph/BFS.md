# BFS

Исследует граф послойно, сначала посещая все вершины на расстоянии 1 от начальной, затем 2 и так далее. Используется очередь.
**Когда использовать:**
- Поиск кратчайшего пути в невзвешанном графе
- Обнаружение компонент связности
- Решение задач связанных с уровнями

{% cut "" %}
```cs
using System;
using System.Collections.Generic;

class Graph
{
    private int vertices;
    private List<int>[] adj;

    public Graph(int v)
    {
        vertices = v;
        adj = new List<int>[v];
        for (int i = 0; i < v; i++)
            adj[i] = new List<int>();
    }

    public void AddEdge(int v, int w)
    {
        adj[v].Add(w);
    }

    public void BFS(int startVertex)
    {
        bool[] visited = new bool[vertices];
        Queue<int> queue = new Queue<int>();

        visited[startVertex] = true;
        queue.Enqueue(startVertex);

        while (queue.Count > 0)
        {
            int v = queue.Dequeue();
            Console.Write(v + " ");

            foreach (int next in adj[v])
            {
                if (!visited[next])
                {
                    visited[next] = true;
                    queue.Enqueue(next);
                }
            }
        }
    }
}

class Program
{
    static void Main()
    {
        Graph g = new Graph(4);
        g.AddEdge(0, 1);
        g.AddEdge(0, 2);
        g.AddEdge(1, 2);
        g.AddEdge(2, 0);
        g.AddEdge(2, 3);
        g.AddEdge(3, 3);

        Console.WriteLine("Обход в ширину, начиная с вершины 2:");
        g.BFS(2); // Вывод: 2 0 3 1
    }
}
```
{% endcut %}