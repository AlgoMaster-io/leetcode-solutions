# 133. [Clone Graph](https://leetcode.com/problems/clone-graph/)

## Approach: Depth-First Search (DFS) with HashMap

### Solution
```cpp
// Time Complexity: O(n), where n is the number of nodes in the graph
// Space Complexity: O(n), for the unordered_map and recursion stack
#include <unordered_map>
#include <vector>

using namespace std;

class Node {
public:
    int val;
    vector<Node*> neighbors;
    Node() : val(0), neighbors(vector<Node*>()) {}
    Node(int _val) : val(_val), neighbors(vector<Node*>()) {}
    Node(int _val, vector<Node*> _neighbors) : val(_val), neighbors(_neighbors) {}
};

class Solution {
private:
    unordered_map<Node*, Node*> map;

public:
    Node* cloneGraph(Node* node) {
        if (node == nullptr) {
            return nullptr;
        }

        // If the node is already cloned, return the cloned node
        if (map.find(node) != map.end()) {
            return map[node];
        }

        // Clone the current node
        Node* clone = new Node(node->val);
        map[node] = clone;

        // Recursively clone all neighbors
        for (Node* neighbor : node->neighbors) {
            clone->neighbors.push_back(cloneGraph(neighbor));
        }

        return clone;
    }
};
```

