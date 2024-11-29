# 834. [Sum of Distances in Tree](https://leetcode.com/problems/sum-of-distances-in-tree/)

## Approach 1: DFS to Calculate Subtree Sizes and Initial Distance

### Solution
```cpp
// Time Complexity: O(n)
// Space Complexity: O(n)
#include <vector>

using namespace std;

class Solution {
public:
    vector<vector<int>> tree;
    vector<int> answer, count;
    
    vector<int> sumOfDistancesInTree(int N, vector<vector<int>>& edges) {
        // Initialize the tree and helper arrays
        tree = vector<vector<int>>(N);
        answer = vector<int>(N, 0);
        count = vector<int>(N, 0);
        
        // Build the tree as an adjacency list
        for (auto& edge : edges) {
            tree[edge[0]].push_back(edge[1]);
            tree[edge[1]].push_back(edge[0]);
        }
        
        // Post-order DFS to calculate count and initial distances
        postOrder(0, -1);
        
        // Pre-order DFS to calculate final answer
        preOrder(0, -1, N);
        
        return answer;
    }
    
private:
    void postOrder(int node, int parent) {
        // Initialize count as 1 (itself)
        count[node] = 1;
        for (int neighbor : tree[node]) {
            if (neighbor == parent) continue; // Avoid going back to parent
            postOrder(neighbor, node);
            // Sum up distances in subtree
            count[node] += count[neighbor];
            answer[node] += answer[neighbor] + count[neighbor];
        }
    }
    
    void preOrder(int node, int parent, int N) {
        for (int neighbor : tree[node]) {
            if (neighbor == parent) continue; // Avoid going back to parent
            // Calculate using previously computed distances
            answer[neighbor] = answer[node] - count[neighbor] + (N - count[neighbor]);
            preOrder(neighbor, node, N);
        }
    }
};
```

This approach uses Depth First Search (DFS) to first calculate the sum of distances from the root to all nodes and uses it to compute the distances for the rest by adjusting them based on the tree structure.

