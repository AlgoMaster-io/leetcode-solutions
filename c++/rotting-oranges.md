## Leetcode Problem: [994. Rotting Oranges](https://leetcode.com/problems/rotting-oranges/)

### Approaches:
- [Approach 1: Breadth-First Search (BFS) with Queue](#approach-1-breadth-first-search-bfs-with-queue)

---

### Approach 1: Breadth-First Search (BFS) with Queue

**Intuition:**

The problem requires us to determine how long it will take for all fresh oranges to rot. This is similar to the concept of the shortest path in an unweighted graph, which can be efficiently solved with a Breadth-First Search (BFS). 

1. Each orange can rot its adjacent fresh oranges in all four directions (up, down, left, right).
2. We can use a queue to hold the current level of rotten oranges and process them one by one.
3. For each processed rotten orange, we will check its adjacent cells. If a fresh orange is found, we rot it and add it to the queue for processing in the next round.
4. Count the number of minutes (or iterations) required till the queue is processed completely.
5. If there are still fresh oranges left after processing the queue, return -1, indicating it is impossible to rot all fresh oranges.

**Steps:**
1. Initialize a queue for BFS and a counter for fresh oranges.
2. Populate the initial queue with the positions of all rotten oranges and count all the fresh oranges.
3. Use BFS to process the queue iteratively.
4. For each rotten orange, attempt to rot adjacent fresh oranges and add them to the queue.
5. Count the time in minutes required to process all the rotten oranges.
6. If there are still fresh oranges left at the end, return -1.

**Time Complexity:** 
- The time complexity is O(n * m) because each cell is processed at most once.

**Space Complexity:** 
- The space complexity is O(n * m) to store the coordinates of rotten oranges in the queue. 

```cpp
#include <vector>
#include <queue>
using namespace std;

int orangesRotting(vector<vector<int>>& grid) {
    // Directions for moving in Up, Down, Left, Right
    vector<pair<int, int>> directions {{-1, 0}, {1, 0}, {0, -1}, {0, 1}};
    
    int rows = grid.size();
    int cols = grid[0].size();
    
    queue<pair<int, int>> rottenQueue; // Queue to hold indices of rotten oranges
    int freshCount = 0; // Counter to track fresh oranges
    
    // Initialize the queue with all starting rotten oranges and count fresh oranges
    for (int r = 0; r < rows; ++r) {
        for (int c = 0; c < cols; ++c) {
            if (grid[r][c] == 2) {
                // Add rotten orange's position to queue
                rottenQueue.push({r, c});
            } else if (grid[r][c] == 1) {
                // Count fresh oranges
                freshCount++;
            }
        }
    }
    
    // If there are no fresh oranges, return 0 as no time is needed
    if (freshCount == 0) {
        return 0;
    }
    
    int minutes = 0; // To track the time in minutes
    
    // Perform BFS
    while (!rottenQueue.empty()) {
        int size = rottenQueue.size();
        bool rotted = false; // To track if any rotting happened in this iteration
        
        for (int i = 0; i < size; ++i) {
            auto [row, col] = rottenQueue.front();
            rottenQueue.pop();
            
            for (auto [dr, dc] : directions) {
                int newRow = row + dr, newCol = col + dc;

                // Check boundaries and that the orange is fresh
                if (newRow >= 0 && newRow < rows && newCol >= 0 && newCol < cols && grid[newRow][newCol] == 1) {
                    // Rot the fresh orange and add its position to the queue
                    grid[newRow][newCol] = 2;
                    rottenQueue.push({newRow, newCol});
                    freshCount--;
                    rotted = true; // We have rotted at least one orange
                }
            }
        }

        // Increment minutes only if any oranges rotted in this iteration
        if (rotted) {
            minutes++;
        }
    }
    
    // If freshCount > 0, some fresh oranges could not be reached
    return (freshCount == 0) ? minutes : -1;
}
```


