# [Leetcode 1584: Min Cost to Connect All Points](https://leetcode.com/problems/min-cost-to-connect-all-points/)

This problem involves connecting all given points with the minimum sum of Manhattan distances. This is conceptually similar to finding the Minimum Spanning Tree (MST) of a graph.

## Solutions

- [Approach 1: Kruskal's Algorithm with Union-Find](#approach-1-kruskals-algorithm-with-union-find)
- [Approach 2: Prim's Algorithm using Priority Queue (Heap)](#approach-2-prims-algorithm-using-priority-queue-heap)

### Approach 1: Kruskal's Algorithm with Union-Find

#### Intuition

Kruskal's algorithm works by sorting all the edges and adding them one by one to the MST. We use a Union-Find (also known as Disjoint Set Union, DSU) data structure to manage and detect cycles. The algorithm ensures only the smallest edges are added, constructing the minimum spanning tree.

Steps:
1. Create a list of all edges along with their associated costs.
2. Sort edges based on their cost.
3. Use a Union-Find structure to progressively add edges, ensuring no cycles are introduced.
4. Stop when we have `n-1` edges (where `n` is the number of points).

#### Code

```csharp
public class Solution
{
    public int MinCostConnectPoints(int[][] points)
    {
        int n = points.Length;
        List<(int cost, int u, int v)> edges = new();
        
        // Generate all edges with their manhattan cost
        for (int i = 0; i < n; i++)
        {
            for (int j = i + 1; j < n; j++)
            {
                int cost = Math.Abs(points[i][0] - points[j][0]) + Math.Abs(points[i][1] - points[j][1]);
                edges.Add((cost, i, j));
            }
        }

        // Sort edges based on the cost
        edges.Sort((a, b) => a.cost.CompareTo(b.cost));
        
        // Initialize Union-Find
        UnionFind uf = new(n);
        int totalCost = 0;
        int edgesUsed = 0;
        
        // Iterate over all sorted edges
        foreach (var (cost, u, v) in edges)
        {
            if (uf.Union(u, v))
            {
                totalCost += cost;
                edgesUsed++;
                if (edgesUsed == n - 1) break; // Stop when we have n-1 edges
            }
        }
        
        return totalCost;
    }
}

public class UnionFind
{
    private int[] parent;
    private int[] rank;
    
    public UnionFind(int size)
    {
        parent = new int[size];
        rank = new int[size];
        for (int i = 0; i < size; i++)
        {
            parent[i] = i;
            rank[i] = 0;
        }
    }
    
    public int Find(int u)
    {
        if (u != parent[u])
            parent[u] = Find(parent[u]); // Path compression
        return parent[u];
    }
    
    public bool Union(int u, int v)
    {
        int rootU = Find(u);
        int rootV = Find(v);
        
        if (rootU == rootV) return false;
        
        // Union by rank
        if (rank[rootU] > rank[rootV])
            parent[rootV] = rootU;
        else if (rank[rootU] < rank[rootV])
            parent[rootU] = rootV;
        else
        {
            parent[rootV] = rootU;
            rank[rootU]++;
        }
        
        return true;
    }
}
```

#### Complexity Analysis
- **Time Complexity**: `O(E log E)`, where `E` is the number of edges. This results from sorting the edges and performing union-find operations.
- **Space Complexity**: `O(E)`, from storing all the edges.

### Approach 2: Prim's Algorithm using Priority Queue (Heap)

#### Intuition

Prim's algorithm starts with a vertex and grows the MST by adding the smallest edge that connects a vertex in the MST to a vertex outside the MST. Using a priority queue helps efficiently retrieve the smallest edge.

Steps:
1. Initialize a priority queue to hold edges prioritized by cost.
2. Start from any point, adding all its adjacent edges to the priority queue.
3. Repeat extracting the smallest edge and add its associated point to the MST; push its connecting edges into the queue.
4. Stop when all points are connected.

#### Code

```csharp
public class Solution
{
    public int MinCostConnectPoints(int[][] points)
    {
        int n = points.Length;
        if (n == 1) return 0;
        
        int totalCost = 0;
        bool[] inMST = new bool[n];
        int edgesUsed = 0;
        
        // PriorityQueue to get the edge with minimum cost
        PriorityQueue<int, int> minHeap = new();
        minHeap.Enqueue(0, 0); // Starting from node 0 with cost 0
        
        while (edgesUsed < n)
        {
            var (currentCost, u) = minHeap.Dequeue();
            
            // Skip if already in MST
            if (inMST[u]) continue;
            
            // Add this vertex to MST
            inMST[u] = true;
            totalCost += currentCost;
            edgesUsed++;
            
            // Add all edges from this vertex to the heap
            for (int v = 0; v < n; v++)
            {
                if (!inMST[v])
                {
                    int nextCost = Math.Abs(points[u][0] - points[v][0]) + Math.Abs(points[u][1] - points[v][1]);
                    minHeap.Enqueue(nextCost, v);
                }
            }
        }
        
        return totalCost;
    }
}
```

#### Complexity Analysis
- **Time Complexity**: `O(E log V)`, where `E` is the number of edges and `V` is the number of vertices (points). This complexity arises from inserting and removing from the priority queue.
- **Space Complexity**: `O(V + E)`, considering the storage for edges in the queue and MST representation.

These approaches are common for constructing minimum spanning trees, effectively utilized to solve problems requiring optimal network connections, like the problem at hand.

