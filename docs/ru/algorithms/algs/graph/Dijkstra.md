# Алгоритм Дейкстры

Для поиска кратчайшего пути во взвешенном графе с неотрицательными весами.

```cs
using System;
using System.Collections.Generic;

class Graph
{
    private int vertices;
    private List<(int, int)>[] adj; // (вершина, вес)

    public Graph(int v)
    {
        vertices = v;
        adj = new List<(int, int)>[v];
        for (int i = 0; i < v; i++)
            adj[i] = new List<(int, int)>();
    }

    public void AddEdge(int v, int w, int weight)
    {
        adj[v].Add((w, weight));
        adj[w].Add((v, weight)); // Для неориентированного графа
    }

    public void Dijkstra(int startVertex)
    {
        int[] dist = new int[vertices];
        for (int i = 0; i < vertices; i++)
            dist[i] = int.MaxValue;

        PriorityQueue<(int, int), int> pq = new PriorityQueue<(int, int), int>();
        dist[startVertex] = 0;
        pq.Enqueue((startVertex, 0), 0);

        while (pq.Count > 0)
        {
            var (vertex, distance) = pq.Dequeue();
            if (distance > dist[vertex]) continue;

            foreach (var (next, weight) in adj[vertex])
            {
                if (dist[vertex] + weight < dist[next])
                {
                    dist[next] = dist[vertex] + weight;
                    pq.Enqueue((next, dist[next]), dist[next]);
                }
            }
        }

        for (int i = 0; i < vertices; i++)
            Console.WriteLine($"Расстояние до вершины {i}: {dist[i]}");
    }
}

// Реализация PriorityQueue для C# (до .NET 6)
public class PriorityQueue<TElement, TPriority>
{
    private List<(TElement, TPriority)> elements = new List<(TElement, TPriority)>();
    public int Count => elements.Count;

    public void Enqueue(TElement element, TPriority priority)
    {
        elements.Add((element, priority));
        elements.Sort((x, y) => Comparer<TPriority>.Default.Compare(x.Item2, y.Item2));
    }

    public TElement Dequeue()
    {
        var element = elements[0].Item1;
        elements.RemoveAt(0);
        return element;
    }
}
```