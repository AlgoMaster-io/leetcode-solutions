# 802. [Find Eventual Safe States](https://leetcode.com/problems/find-eventual-safe-states/)

## Approach 1: Graph Coloring

### Solution
java
```java
// Time Complexity: O(V + E)
// Space Complexity: O(V)
import java.util.ArrayList;
import java.util.List;

public class Solution {
    public List<Integer> eventualSafeNodes(int[][] graph) {
        int n = graph.length;
        int[] color = new int[n];
        List<Integer> result = new ArrayList<>();

        for (int i = 0; i < n; i++) {
            if (isSafeNode(graph, color, i)) {
                result.add(i);
            }
        }
        
        return result;
    }

    private boolean isSafeNode(int[][] graph, int[] color, int node) {
        if (color[node] > 0) {
            return color[node] == 2;
        }

        color[node] = 1; // Mark the node temporarily (GRAY)

        for (int nextNode : graph[node]) {
            if (!isSafeNode(graph, color, nextNode)) {
                return false;
            }
        }

        color[node] = 2; // Mark the node as safe (BLACK)
        return true;
    }
}
```

### Solution
c++
```cpp
// Time Complexity: O(V + E)
// Space Complexity: O(V)
#include <vector>
using namespace std;

class Solution {
public:
    vector<int> eventualSafeNodes(vector<vector<int>>& graph) {
        int n = graph.size();
        vector<int> color(n, 0);
        vector<int> result;

        for (int i = 0; i < n; i++) {
            if (isSafeNode(graph, color, i)) {
                result.push_back(i);
            }
        }

        return result;
    }

private:
    bool isSafeNode(vector<vector<int>>& graph, vector<int>& color, int node) {
        if (color[node] > 0) {
            return color[node] == 2;
        }

        color[node] = 1; // Mark the node temporarily (GRAY)

        for (int nextNode : graph[node]) {
            if (!isSafeNode(graph, color, nextNode)) {
                return false;
            }
        }

        color[node] = 2; // Mark the node as safe (BLACK)
        return true;
    }
};
```

