# [Leetcode 1514: Path with Maximum Probability](https://leetcode.com/problems/path-with-maximum-probability/)

## Solutions

- [Approach 1: Dijkstra's Algorithm Using Min-Heap](#approach-1-dijkstras-algorithm-using-min-heap)
- [Approach 2: Modified Bellman-Ford Algorithm](#approach-2-modified-bellman-ford-algorithm)

### Approach 1: Dijkstra's Algorithm Using Min-Heap

**Intuition**

Dijkstra's algorithm is typically used to find the shortest path in a graph. However, it can be modified to find the path with the maximum probability by changing the way we update our "distances". Instead of minimizing distance, we maximize probability.

**Steps**:

1. Use a max-heap to keep track of the path with the maximum probability. Initialize with a probability of 1 (i.e., 100%) for the start node.
2. For the current node, update the probability to each of its neighbors.
3. If we find a better probability (larger than previously recorded), add that neighbor to the max-heap.
4. Continue the process till the heap is empty or the target node is reached.

**Python Code**:

```python
import heapq
from collections import defaultdict

def maxProbability(n, edges, succProb, start, end):
    graph = defaultdict(list)
    # Construct the graph
    for (u, v), prob in zip(edges, succProb):
        graph[u].append((v, prob))
        graph[v].append((u, prob))

    # Max-heap to store (negative_of_prob, vertex) since heapq is a min-heap
    heap = [(-1, start)]
    # Probability of reaching each node
    probabilities = [0] * n
    probabilities[start] = 1  # Start node has a probability of 1 to start

    while heap:
        prob, node = heapq.heappop(heap)
        prob = -prob  # Convert back to positive since we stored as negative

        if node == end:
            return prob

        for neighbor, edge_prob in graph[node]:
            # Calculate new probability
            new_prob = prob * edge_prob
            # If we can reach neighbor with a higher probability, update it
            if new_prob > probabilities[neighbor]:
                probabilities[neighbor] = new_prob
                heapq.heappush(heap, (-new_prob, neighbor))

    return 0.0  # If the end is not reached

```

**Time Complexity**: \(O(E \log V)\), where \(E\) is the number of edges and \(V\) is the number of vertices. In the worst case, we process each edge with a logarithmic operation due to the heap.

**Space Complexity**: \(O(V + E)\) for storing the graph and heap.

### Approach 2: Modified Bellman-Ford Algorithm

**Intuition**

The Bellman-Ford algorithm iteratively relaxes edges and is typically used to find shortest paths in a graph even with negative weights. For this problem, we can adapt it to also maximize the probability by updating the probability for the paths rather than distances.

**Steps**:

1. Initialize a probabilities array with 0, except the start node which is 1.
2. Relax all edges up to \(n-1\) times (where \(n\) is the number of nodes).
3. Update the probability for each node if a higher probability path is found.
4. After \(n-1\) iterations, check the probability of the end to get the result.

**Python Code**:

```python
def maxProbability(n, edges, succProb, start, end):
    # Initialize probability of reaching each node to 0
    probabilities = [0.0] * n
    # Start node has a probability of 1
    probabilities[start] = 1.0

    # Relax edges up to n-1 times
    for _ in range(n - 1):
        for (u, v), prob in zip(edges, succProb):
            # Relaxation step
            if probabilities[u] * prob > probabilities[v]:
                probabilities[v] = probabilities[u] * prob
            if probabilities[v] * prob > probabilities[u]:
                probabilities[u] = probabilities[v] * prob

    return probabilities[end]

```

**Time Complexity**: \(O(V \times E)\), where \(V\) is the number of vertices and \(E\) is the number of edges. We iterate over all edges up to \(n-1\) times.

**Space Complexity**: \(O(V)\) for storing the probabilities of each node.

These approaches provide different methodologies to approach the problem with Dijkstra offering a more optimal solution for denser graphs.

