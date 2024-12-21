# [752. Open the Lock](https://leetcode.com/problems/open-the-lock/)

## Solutions

1. [Breadth-First Search (BFS) Approach](#bfs-approach)

---

### Breadth-First Search (BFS) Approach

**Intuition**:  
The problem can be visualized as a shortest path problem in an unweighted graph, with each node representing a lock combination and edges representing one-step moves (by rotating a wheel one notch). BFS is suitable for finding the shortest path in such graphs because it explores all nodes at the present depth prior to moving on to nodes at the next depth level.

1. Start from the initial lock position "0000".
2. Use a queue to facilitate BFS traversal.
3. Apply a set to track visited combinations to avoid revisiting and unnecessary loops.
4. Each position in the lock can be moved upward or downward, resulting in 8 possible new positions from any current position.
5. Avoid positions listed as 'deadends' immediately.
6. Count the level number to denote the depth of current node exploration, representing the number of moves made.
7. The first time the target combination is encountered, the number of steps is returned.

**C++ Implementation**:

```cpp
#include <iostream>
#include <vector>
#include <string>
#include <queue>
#include <unordered_set>

using namespace std;

int openLock(vector<string>& deadends, string target) {
    unordered_set<string> dead(deadends.begin(), deadends.end());
    unordered_set<string> visited;
    queue<string> q;
    int steps = 0;
    
    // Start from '0000'
    if (dead.find("0000") != dead.end()) 
        return -1; // Initial position is deadend

    q.push("0000");
    visited.insert("0000");
    
    while (!q.empty()) {
        int size = q.size();
        
        // Process current level
        for (int i = 0; i < size; ++i) {
            string current = q.front();
            q.pop();
            
            // Check if current combination is the target
            if (current == target) 
                return steps;
            
            // Try all possible moves
            for (int j = 0; j < 4; ++j) {
                string next = current;
                // Move wheel one step forward
                next[j] = (next[j] - '0' + 1) % 10 + '0';
                if (visited.find(next) == visited.end() && dead.find(next) == dead.end()) {
                    q.push(next);
                    visited.insert(next);
                }
                
                // Reset next for backward move
                next = current;
                // Move wheel one step backward
                next[j] = (next[j] - '0' - 1 + 10) % 10 + '0';
                if (visited.find(next) == visited.end() && dead.find(next) == dead.end()) {
                    q.push(next);
                    visited.insert(next);
                }
            }
        }
        steps++;
    }
    return -1; // If target is unreachable
}

// Example usage
int main() {
    vector<string> deadends = {"0201","0101","0102","1212","2002"};
    string target = "0202";
    cout << openLock(deadends, target) << endl; // Output: 6
    return 0;
}
```

**Time Complexity**: O(10^4 + D) where D is the number of deadends. The worst-case scenario can potentially explore all combinations (10000).

**Space Complexity**: O(10^4 + D). This includes storage for the queue and visited set, which could grow to at most 10000 elements (all lock combinations) and includes D elements for deadends.

