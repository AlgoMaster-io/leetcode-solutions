# [Leetcode 753: Cracking the Safe](https://leetcode.com/problems/cracking-the-safe/)

## Approaches:
- [Approach 1: DFS with Backtracking](#approach-1-dfs-with-backtracking)

### Approach 1: DFS with Backtracking

**Intuition:**

The problem essentially boils down to generating a sequence that contains every possible combination of digits of length `n` exactly once. One intuitive way to achieve this is by using a graph-based approach where each possible combination of digits of length `n-1` is a node. Each node has edges representing appending a digit from 0 to `k-1`. Using DFS, we can traverse this graph, ensuring that we only visit each edge (combination) once, effectively giving us a sequence that is Eulerian.

The key insight is to use the properties of an Eulerian Path (or Circuit) which states that, in a directed graph, a cycle exists that visits every edge exactly once.

**Steps:**

1. Create a starting point consisting of `n-1` zeros.
2. Utilize DFS to explore each node and try all possible next digits.
3. For each node (combination), mark it as visited and concatenate the new digit to the result.
4. If we’ve visited all possible combinations, output the result.

```cpp
#include <iostream>
#include <vector>
#include <unordered_set>
#include <string>

using namespace std;

class Solution {
public:
    string crackSafe(int n, int k) {
        // Start from a string of n-1 '0's
        string start = string(n - 1, '0');
        unordered_set<string> visited;
        string result;
        
        // Initialize the starting node in the Eulerian path
        dfs(start, visited, result, k, n);
        
        // Append the start node to close the cycle
        return result + start;
    }
    
private:
    void dfs(const string& node, unordered_set<string>& visited, string& result, int k, int n) {
        // Traverse every possible next digit
        for (int i = 0; i < k; ++i) {
            string next = node + char('0' + i);
            if (visited.find(next) == visited.end()) {
                visited.insert(next);
                // Recur for next node
                dfs(next.substr(1), visited, result, k, n);
                result.push_back('0' + i);  // Append to result on retreat
            }
        }
    }
};
```

**Time Complexity:**

- O(k^n): There are k^n possible combinations of numbers, and each needs to be visited once.

**Space Complexity:**

- O(k^n): We maintain a hash set for visited nodes which can grow to k^n, and continual depth recursions for DFS also result in additional space used by the call stack.

This approach leverages the properties of Eulerian paths in graph theory to efficiently generate the required sequence, ensuring that the generated sequence visits every possible combination exactly once.

