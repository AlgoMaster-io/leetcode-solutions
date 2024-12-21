# [Cracking the Safe](https://leetcode.com/problems/cracking-the-safe/)

## Approaches
- [Approach 1: Brute Force Simulation](#approach-1-brute-force-simulation)
- [Approach 2: Hierholzer's Algorithm for Eulerian Path](#approach-2-hierholzers-algorithm-for-eulerian-path)

### Approach 1: Brute Force Simulation

#### Intuition

The brute force approach attempts to generate every possible password of the given length `n` for a keypad with `k` digits. We simulate the process of entering each possible combination in the lock and check whether the combination has been visited before. This method lacks efficiency but is a good start for understanding the problem.

#### Steps
1. Start with a sequence having several `n` zeros since we assume the lock starts at `000...`.
2. Use a recursive function to simulate adding digits from `0` to `k-1`.
3. For each combination, check if it has already been visited.
4. If it hasn't, add it to the result path.
5. Continue until every possible combination has been visited.

```javascript
function crackSafe(n, k) {
    const visited = new Set();
    let res = '0'.repeat(n - 1);

    const totalCombinations = Math.pow(k, n);
    
    const dfs = (node) => {
        for (let i = 0; i < k; i++) {
            const next = node + i;
            if (!visited.has(next)) {
                visited.add(next);
                dfs(next.slice(1));
                res += i;
            }
        }
    };
    
    dfs(res);
    res = res.slice(0, totalCombinations + n - 1);
    return res;
}
```

#### Time Complexity
- **Time:** O(k^n * n), where n is the length of the password and k is the number of digits.
- **Space:** O(n * k^n), for storing all the combinations.

### Approach 2: Hierholzer's Algorithm for Eulerian Path

#### Intuition

This approach leverages the concept of an Eulerian path in graphs. The idea is to construct the De Bruijn sequence for the keypad. This path visits every possible combination of nodes exactly once. This gives a more optimal solution for the problem.

#### Steps
1. Start with an arbitrary node represented by `0` repeated `n-1` times.
2. Traverse the graph using a depth-first search (DFS).
3. Append the last digit to the result to form a sequence.
4. This path will ensure that all combinations are visited exactly once.

```javascript
function crackSafe(n, k) {
    const visited = new Set();
    let result = '';
    
    // Start with a node of n-1 zeroes, since that's the first initial node
    const start = '0'.repeat(n - 1);
    
    const dfs = (node) => {
        for (let i = 0; i < k; i++) {
            const next = node + i;
            if (!visited.has(next)) {
                visited.add(next);
                dfs(next.slice(1)); // Move the window 1 to the right
                result += i;        // Append the current digit after visiting all continuations
            }
        }
    };
    
    dfs(start);
    result += start; // Append the initial node since it was used to start off the DFS
    return result;
}
```

#### Time Complexity
- **Time:** O(k^n), where n is the length of the password and k is the number of digits, due to visiting each edge once.
- **Space:** O(n * k^n), for the visited set and call stack.

This approach is more efficient than the brute-force method, minimizing redundant operations.

