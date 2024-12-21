# [Leetcode 753: Cracking the Safe](https://leetcode.com/problems/cracking-the-safe/)

## Approaches:
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Hierholzer's Algorithm for Eulerian Path](#approach-2-hierholzers-algorithm-for-eulerian-path)

## Approach 1: Brute Force

### Intuition:
The brute force approach examines all possible permutations of the sequence. Given the size constraints, we aim to generate all possible combinations of the given alphabet size and `n` digits and verify which sequence covers all these combinations. This method is highly inefficient for larger values but introduces the problem fundamentals.

### Steps:
1. Generate all possible combinations using the available digits, creating a set of these combinations to be covered.
2. Start with any sequence filling the initial part with the same digit, and proceed one step at a time rechecking for repeats.
3. Continue updating the sequence until all combinations have been covered.

### Code:
```typescript
function crackSafe(n: number, k: number): string {
    // Set to hold all the combinations that need to be covered
    const combinations = new Set<string>();

    // Generate all combinations by creating an initial sequence of '000..' of length n
    const buildAllCombinations = (combo: string) => {
        if (combo.length === n) {
            combinations.add(combo);
            return;
        }
        for (let i = 0; i < k; i++) {
            buildAllCombinations(combo + i);
        }
    };

    buildAllCombinations('');

    // Start with all zeros sequence
    let sequence = '0'.repeat(n);
    const usedSet = new Set<string>();
    usedSet.add(sequence);

    // Keep track of total digits needed 
    const totalDigits = Math.pow(k, n) + (n - 1);

    while (sequence.length < totalDigits) {
        for (let i = k - 1; i >= 0; i--) {
            const next = sequence.slice(1 - n) + i;

            // Verify if the current next combination has been already used or not
            if (!usedSet.has(next)) {
                usedSet.add(next);
                sequence += i.toString();
                break;
            }
        }
    }

    return sequence;
}
```

### Time Complexity:
- O(k^n * n), where n is the total digits and k is the alphabet size. Each combination of `n` is visited.
### Space Complexity:
- O(k^n), as all combinations need to be stored.

## Approach 2: Hierholzer's Algorithm for Eulerian Path

### Intuition:
The optimal solution involves constructing an Eulerian path in a de Bruijn graph. Hierholzer’s algorithm finds an Eulerian path by traversing the graph's edges precisely once, starting from any vertex with unused edges. Here, vertices represent sequences, and edges represent possible state transitions.

### Steps:
1. Build the de Bruijn graph representing each `n-1` length prefix as nodes and possible suffixes as edges.
2. Construct the Eulerian path using Hierholzer’s algorithm, ensuring each edge is used once, producing the desired sequence.
3. Start from an initial node (e.g., "000...") and repeatedly follow unused edges, backtracking when needed until all edges are visited.

### Code:
```typescript
function crackSafe(n: number, k: number): string {
    // Initialize a result sequence with starting node '000...'
    let result = '0'.repeat(n);

    // Create a set to track the visited edges
    const visited = new Set<string>();
    
    const totalNodes = Math.pow(k, n); // The total number of nodes/edges in graph

    // Hierholzer's algorithm for finding the Eulerian path
    const dfs = (node: string) => {
        for (let i = 0; i < k; i++) {
            const edge = node + i;
            
            // Ensure this edge is not visited
            if (!visited.has(edge)) {
                visited.add(edge);
                dfs(edge.slice(1)); // Continue the search from the new node
                result += i.toString();
            }
        }
    };

    // Start DFS from the initial node
    dfs('0'.repeat(n - 1));

    return result;
}
```

### Time Complexity:
- O(k^n), because each edge is visited once in the Eulerian circuit.
### Space Complexity:
- O(k^n), used to maintain the visited set of edges. 

In conclusion, Hierholzer's algorithm offers an efficient path for solving the problem by leveraging properties of Eulerian circuits and the structure of de Bruijn graphs. This approach balances time complexity optimally across potential solutions.

