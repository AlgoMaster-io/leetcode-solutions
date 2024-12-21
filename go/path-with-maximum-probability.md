# [Leetcode 1514: Path with Maximum Probability](https://leetcode.com/problems/path-with-maximum-probability/)

## Approaches
1. [Basic Solution: Dijkstra-like BFS](#basic-solution:-dijkstra-like-bfs)
2. [Optimal Solution: Dijkstra Algorithm using Max-Heap](#optimal-solution:-dijkstra-algorithm-using-max-heap)

### Basic Solution: Dijkstra-like BFS

**Intuition**
To solve the problem of finding the path with the maximum probability, we can use a Breadth-First Search (BFS) approach similar to Dijkstra's algorithm. Dijkstra's algorithm typically finds the shortest path; however, our goal here is to maximize the probability. Since probabilities are multiplicative, maximizing them is similar to finding the path with the maximum "weight" if we take `-log(probability)` as the weight, which is equivalent to multiplying probabilities directly.

**Approach:**
1. Use a queue for BFS starting from the source node.
2. Maintain a probability array to store the maximum probability of reaching each node.
3. For each node, update its neighbors with the new probability if it's higher than the current stored probability.
4. Continue BFS until all possible nodes are processed.

**Code:**

```go
func maxProbability(n int, edges [][]int, succProb []float64, start int, end int) float64 {
    // Create the adjacency list
    adjList := make([][]struct{node int; prob float64}, n)
    for i, edge := range edges {
        u, v := edge[0], edge[1]
        prob := succProb[i]
        adjList[u] = append(adjList[u], struct{node int; prob float64}{v, prob})
        adjList[v] = append(adjList[v], struct{node int; prob float64}{u, prob})
    }
    
    // Array to store the maximum probability to reach each node
    prob := make([]float64, n)
    prob[start] = 1.0
    
    // Queue for BFS
    queue := []int{start}
    
    for len(queue) > 0 {
        current := queue[0]
        queue = queue[1:]
        
        // Traverse all neighbors of the current node
        for _, neighbor := range adjList[current] {
            // Calculate the new probability to reach the neighbor
            newProb := prob[current] * neighbor.prob
            // If this probability is greater than the known probability, update it
            if newProb > prob[neighbor.node] {
                prob[neighbor.node] = newProb
                queue = append(queue, neighbor.node)
            }
        }
    }
    
    return prob[end]
}
```

**Complexity Analysis:**
- **Time Complexity:** O(V + E), where V is the number of vertices and E is the number of edges. We effectively perform a BFS traversal of the graph.
- **Space Complexity:** O(V + E) for storing the adjacency list and the queue.

### Optimal Solution: Dijkstra Algorithm using Max-Heap

**Intuition**
The BFS approach can be enhanced using a priority queue (a max-heap) as in the traditional Dijkstra algorithm but modified to maximize probability rather than minimize distance. This approach will ensure that at each step, we process the node with the highest achievable probability, making it more efficient.

**Approach:**
1. Use a max-heap (priority queue) to always expand the most promising node (node with the highest probability) first.
2. Maintain a probability array to store the maximum probability for reaching each node.
3. Start from the source with a probability of 1, pushing it into the heap.
4. For each node expand its neighbors, updating their probabilities and pushing them into the heap if having a higher probability.

**Code:**

```go
import "container/heap"

// Define a priority queue item
type Item struct {
    node int
    prob float64
}

// A max-heap for the priority queue
type PriorityQueue []*Item

func (pq PriorityQueue) Len() int { return len(pq) }

func (pq PriorityQueue) Less(i, j int) bool { return pq[i].prob > pq[j].prob }

func (pq PriorityQueue) Swap(i, j int) { pq[i], pq[j] = pq[j], pq[i] }

func (pq *PriorityQueue) Push(x interface{}) { *pq = append(*pq, x.(*Item)) }

func (pq *PriorityQueue) Pop() interface{} {
    old := *pq
    n := len(old)
    item := old[n-1]
    *pq = old[0 : n-1]
    return item
}

func maxProbability(n int, edges [][]int, succProb []float64, start int, end int) float64 {
    // Create the adjacency list
    adjList := make([][]struct{node int; prob float64}, n)
    for i, edge := range edges {
        u, v := edge[0], edge[1]
        prob := succProb[i]
        adjList[u] = append(adjList[u], struct{node int; prob float64}{v, prob})
        adjList[v] = append(adjList[v], struct{node int; prob float64}{u, prob})
    }
    
    // Array to store the maximum probability to reach each node
    prob := make([]float64, n)
    prob[start] = 1.0
    
    // Create a max-heap
    pq := &PriorityQueue{}
    heap.Init(pq)
    heap.Push(pq, &Item{start, 1.0})
    
    for pq.Len() > 0 {
        currentItem := heap.Pop(pq).(*Item)
        currentNode := currentItem.node
        currentProb := currentItem.prob
        
        if currentNode == end {
            return currentProb
        }
        
        // Traverse all neighbors of the current node
        for _, neighbor := range adjList[currentNode] {
            newProb := currentProb * neighbor.prob
            if newProb > prob[neighbor.node] {
                prob[neighbor.node] = newProb
                heap.Push(pq, &Item{neighbor.node, newProb})
            }
        }
    }
    
    return 0.0
}
```

**Complexity Analysis:**
- **Time Complexity:** O((V + E) log V), due to the heap operations for each vertex.
- **Space Complexity:** O(V + E) for storing the adjacency list and for the priority queue.

