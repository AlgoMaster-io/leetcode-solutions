# [Leetcode 753: Cracking the Safe](https://leetcode.com/problems/cracking-the-safe/)

## Approaches
- [Approach 1: Backtracking with Pruning](#approach-1-backtracking-with-pruning)
- [Approach 2: Hierholzer's Algorithm (Eulerian Path)](#approach-2-hierholzers-algorithm-eulerian-path)

---

## Approach 1: Backtracking with Pruning

**Intuition:**

The problem can initially be approached using a backtracking strategy. The goal is to explore all possible combinations of the digits by constructing a sequence that includes every k-length combination of digits from `0` to `n-1`.

1. Start with an initial sequence and attempt to add additional digits to the sequence while ensuring that all combinations are attempted.
2. Prune paths that develop to a duplicate sequence such that we do not repeat work previously done.

**Steps:**

- Begin with an initial sequence of `k-1` zeroes (i.e., `'00...0'`).
- Attempt to append every possible digit (`0` to `n-1`) to the current sequence.
- Keep track of visited sequences to avoid repeats using a set.
- If a new sequence is not visited, mark it as visited and continue searching.
- Backtrack if necessary when all options have been explored.

```python
def crackSafe(n: int, k: int) -> str:
    # Start with '0' * (n-1) i.e., "000... (n-1 times)"
    start = '0' * (n - 1)
    visited = set()
    # The final sequence should have all possible k combinations of length n
    result = []
    
    def dfs(node):
        for x in map(str, range(k)):
            # Create a new node by appending digit x
            neighbor = node + x
            if neighbor not in visited:
                visited.add(neighbor)
                # Recursively build further from this node
                dfs(neighbor[1:])
                # Add the last digit of neighbor to result
                result.append(x)

    dfs(start)
    return start + ''.join(result)

"""
Time Complexity: O(k^n). We visit every possible state (which is k^n). The recursion unfolds with each state having k potential children.
Space Complexity: O(k^n). We store each visited state exactly once and our path result will also roughly be of length k^n.
"""

```

## Approach 2: Hierholzer's Algorithm (Eulerian Path)

**Intuition:**

The most optimal solution involves applying Hierholzer's Algorithm, a well-known method to find an Eulerian path in a graph. Construct a directed graph, for every possible k-length number, create edges to its subsequent states.

1. Treat each k-length sequence as a node in the graph.
2. Decode the sequence exploration as moving through a graph path ensuring that node transitions form an Eulerian circuit.
3. The problem is transformed into finding such a path that visits every edge (sequence) exactly once.

**Steps:**

- Use a stack to perform a depth-first traversal of the graph and track path formation.
- Upon returning from a node (backtracking), we append to the path sequence.
- Essentially, we form the sequence by the Eulerian path we construct, ensuring no repeated traversal of any edge.

```python
def crackSafe(n: int, k: int) -> str:
    total_combinations = k ** n
    visited = set()
    result = []

    def dfs(node):
        for i in map(str, range(k)):
            neighbor = node + i
            if neighbor not in visited:
                visited.add(neighbor)
                dfs(neighbor[1:])
                result.append(i)
    
    # Start at node '0' * (n-1) like before since we're attempting to complete the circuit
    start = '0' * (n - 1)
    dfs(start)
    return start + ''.join(result)

"""
Time Complexity: O(k^n). We find the Eulerian circuit by visiting each edge exactly once.
Space Complexity: O(k^n). Storage for visited set and resultant Eulerian circuit path.
"""

```

In both approaches, complex exploration of number sequences is simplified using graph traversal concepts, thereby enhancing the efficiency of sequence formation. Approach 2, with a focus on Eulerian paths, offers an elegant and direct solution.

