# DFS

Исследует граф продвигаясь как можно глубже по каждому пути прежде чем возвращаться. Использует стек (явный или через рекурсию).
**Когда использовать:**
- Поиск путей
- Обнаружение циклов
- Топологическая сортировка
- Поиск компонент связности

{% cut "Рекурсивный DFS" %}
```cs
using System;
using System.Collections.Generic;

class Graph
{
    private int vertices; // Количество вершин
    private List<int>[] adj; // Список смежности

    public Graph(int v)
    {
        vertices = v;
        adj = new List<int>[v];
        for (int i = 0; i < v; i++)
            adj[i] = new List<int>();
    }

    public void AddEdge(int v, int w)
    {
        adj[v].Add(w); // Добавление ребра v -> w
    }

    public void DFS(int v, bool[] visited)
    {
        visited[v] = true; // Помечаем вершину как посещённую
        Console.Write(v + " ");

        foreach (int next in adj[v])
        {
            if (!visited[next])
                DFS(next, visited); // Рекурсивно обходим смежные вершины
        }
    }

    public void StartDFS(int startVertex)
    {
        bool[] visited = new bool[vertices];
        DFS(startVertex, visited);
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

        Console.WriteLine("Обход в глубину, начиная с вершины 2:");
        g.StartDFS(2); // Вывод: 2 0 1 3
    }
}
```
{% endcut %}

{% cut "Итеративный DFS" %}
```cs
public void IterativeDFS(int startVertex)
{
    bool[] visited = new bool[vertices];
    Stack<int> stack = new Stack<int>();
    stack.Push(startVertex);

    while (stack.Count > 0)
    {
        int v = stack.Pop();
        if (!visited[v])
        {
            Console.Write(v + " ");
            visited[v] = true;
        }

        foreach (int next in adj[v])
        {
            if (!visited[next])
                stack.Push(next);
        }
    }
}
```
{% endcut %}