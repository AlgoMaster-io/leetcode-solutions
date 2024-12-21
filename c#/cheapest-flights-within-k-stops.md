# [787. Cheapest Flights Within K Stops](https://leetcode.com/problems/cheapest-flights-within-k-stops/)

## Table of Contents
- [Approach 1: DFS with Pruning](#approach-1)
- [Approach 2: Bellman-Ford Algorithm](#approach-2)
- [Approach 3: Dijkstra's Algorithm (Priority Queue)](#approach-3)

## Approach 1: DFS with Pruning

### Intuition:
Use Depth First Search (DFS) to explore all possible paths from the source to the destination. Prune the search if the number of edges exceeds `k`. Keep track of the minimum cost in a globally shared variable.

### Code:

```csharp
public class Solution
{
    private int minCost = int.MaxValue;
    
    public int FindCheapestPrice(int n, int[][] flights, int src, int dst, int K) 
    {
        var graph = new Dictionary<int, List<(int next, int price)>>();
        
        // Build the adjacency list representation of the graph
        foreach (var flight in flights)
        {
            if (!graph.ContainsKey(flight[0]))
                graph[flight[0]] = new List<(int, int)>();
            
            graph[flight[0]].Add((flight[1], flight[2]));
        }
        
        DFS(graph, src, dst, K+1, 0);
        
        return minCost == int.MaxValue ? -1 : minCost;
    }
    
    private void DFS(Dictionary<int, List<(int next, int price)>> graph, int node, int dst, int stops, int cost)
    {
        if (node == dst)
        {
            minCost = Math.Min(minCost, cost);
            return;
        }
        
        if (stops == 0)
            return;
        
        if (!graph.ContainsKey(node))
            return;
        
        foreach (var next in graph[node])
        {
            if (cost + next.price > minCost)
                continue;
            
            DFS(graph, next.next, dst, stops - 1, cost + next.price);
        }
    }
}
```
### Time Complexity:
- O(n^k): In the worst case, DFS explores all possible paths.

### Space Complexity:
- O(n): For the recursion stack in the worst case.

## Approach 2: Bellman-Ford Algorithm

### Intuition:
Use Bellman-Ford algorithm that finds shortest paths with up to `k` edges. This algorithm systematically relaxes all the flight edges `k+1` times.

### Code:

```csharp
public class Solution
{
    public int FindCheapestPrice(int n, int[][] flights, int src, int dst, int K) 
    {
        int[] prices = new int[n];
        Array.Fill(prices, int.MaxValue);
        prices[src] = 0;

        for (int i = 0; i <= K; i++)
        {
            int[] tempPrices = (int[])prices.Clone();
            
            foreach (var flight in flights)
            {
                int u = flight[0], v = flight[1], price = flight[2];
                
                if (prices[u] == int.MaxValue)
                    continue;
                
                if (prices[u] + price < tempPrices[v])
                {
                    tempPrices[v] = prices[u] + price;
                }
            }
            
            prices = tempPrices;
        }

        return prices[dst] == int.MaxValue ? -1 : prices[dst];
    }
}
```
### Time Complexity:
- O(k * e): `k` iterations over all `e` edges.

### Space Complexity:
- O(n): Temporary space to hold the prices.

## Approach 3: Dijkstra's Algorithm (Priority Queue)

### Intuition:
Use a priority queue to keep track of least cost nodes to explore. Each entry in the priority queue consists of a tuple (current cost, node, remaining stops). It efficiently explores nodes with the lowest current cost and prunes paths longer than `k`.

### Code:

```csharp
public class Solution
{
    public int FindCheapestPrice(int n, int[][] flights, int src, int dst, int K) 
    {
        var graph = new Dictionary<int, List<(int next, int price)>>();
        
        foreach (var flight in flights)
        {
            if (!graph.ContainsKey(flight[0]))
                graph[flight[0]] = new List<(int, int)>();
            
            graph[flight[0]].Add((flight[1], flight[2]));
        }
        
        PriorityQueue<(int cost, int node, int stops), int> pq = new PriorityQueue<(int, int, int), int>();
        pq.Enqueue((0, src, K + 1), 0);

        while (pq.Count > 0)
        {
            var (cost, node, stops) = pq.Dequeue();
            
            if (node == dst)
                return cost;
                
            if (stops > 0)
            {
                if (!graph.ContainsKey(node))
                    continue;
                
                foreach (var (next, price) in graph[node])
                {
                    pq.Enqueue((cost + price, next, stops - 1), cost + price);
                }
            }
        }

        return -1;
    }
}
```

### Time Complexity:
- O((n + e) log n): Uses a priority queue to efficiently get the node with the least cost, considering up to `k` stops.

### Space Complexity:
- O(n + e): Storing nodes and edges as a graph representation.

Each of these methods provides a unique approach to handle the problem, allowing you to select based on the problem requirements and constraints.

