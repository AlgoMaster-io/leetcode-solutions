# 752. [Open the Lock](https://leetcode.com/problems/open-the-lock/)

## Approach: Breadth-First Search (BFS)

### Solution
```cpp
// Time Complexity: O(10^4 + d), where d is the size of the deadends
// Space Complexity: O(10^4 + d), for the visited set and the queue
#include <unordered_set>
#include <queue>
#include <string>
#include <vector>

class Solution {
public:
    int openLock(std::vector<std::string>& deadends, std::string target) {
        std::unordered_set<std::string> deadSet(deadends.begin(), deadends.end());

        if (deadSet.find("0000") != deadSet.end()) {
            return -1; // Cannot start from the initial position
        }

        std::queue<std::string> queue;
        std::unordered_set<std::string> visited;
        queue.push("0000");
        visited.insert("0000");

        int moves = 0;

        // Perform BFS
        while (!queue.empty()) {
            int size = queue.size();

            for (int i = 0; i < size; i++) {
                std::string current = queue.front(); queue.pop();

                if (current == target) {
                    return moves; // Target reached
                }

                // Generate all possible next combinations
                for (int j = 0; j < 4; j++) {
                    char c = current[j];

                    // Turn the wheel one step forward
                    std::string forward = current;
                    forward[j] = (c == '9') ? '0' : c + 1;
                    if (deadSet.find(forward) == deadSet.end() && visited.find(forward) == visited.end()) {
                        queue.push(forward);
                        visited.insert(forward);
                    }

                    // Turn the wheel one step backward
                    std::string backward = current;
                    backward[j] = (c == '0') ? '9' : c - 1;
                    if (deadSet.find(backward) == deadSet.end() && visited.find(backward) == visited.end()) {
                        queue.push(backward);
                        visited.insert(backward);
                    }
                }
            }

            moves++;
        }

        return -1; // Target is unreachable
    }
};
```

