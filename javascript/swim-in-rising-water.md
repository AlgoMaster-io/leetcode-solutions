# [Leetcode 778: Swim in Rising Water](https://leetcode.com/problems/swim-in-rising-water/)

### Solutions:

1. [Brute Force Approach](#brute-force-approach)
2. [Using Min-Heap (Priority Queue)](#using-min-heap-priority-queue)
3. [Binary Search with BFS](#binary-search-with-bfs)

### Brute Force Approach

**Intuition:**

The brute force approach involves checking all possible paths from the starting corner of the grid `(0,0)` to the opposite corner `(N-1,N-1)`. We simulate the situation where the water level rises by 1 in each time step and check if there’s a path available under this water level condition. If there is a path, we try to check if it is possible at a lower water level. This approach generally tries all scenarios and is inefficient due to the exponential number of possible paths.

**Code:**

```javascript
function swimInWater(grid) {
    const n = grid.length;
    const direction = [[1, 0], [-1, 0], [0, 1], [0, -1]];

    function canSwim(t) {
        const visited = Array.from({ length: n }, () => Array(n).fill(false));
        const queue = [[0, 0]];
        visited[0][0] = true;
        
        while (queue.length > 0) {
            const [x, y] = queue.pop();
            
            if (x === n - 1 && y === n - 1) return true;
            
            for (let [dx, dy] of direction) {
                const nx = x + dx, ny = y + dy;
                if (
                    nx >= 0 && nx < n && ny >= 0 && ny < n &&
                    !visited[nx][ny] && grid[nx][ny] <= t
                ) {
                    visited[nx][ny] = true;
                    queue.push([nx, ny]);
                }
            }
        }
        return false;
    }
  
    let time = 0;
    while (!canSwim(time)) {
        time++;
    }
    return time;
}
```

**Time Complexity:** O((N^2) * N), where N is the dimension of the grid.

**Space Complexity:** O(N^2), for the visited grid and queue.

### Using Min-Heap (Priority Queue)

**Intuition:**

We can improve efficiency using a min-heap (priority queue). The key is that we wish to minimize the "maximum water level" we encounter on the path to our destination `(N-1, N-1)`. At each step, expand the least-time cell available from the heap and include its adjacent cells in our consideration. 

**Code:**

```javascript
function swimInWater(grid) {
    const n = grid.length;
    const heap = new MinPriorityQueue({priority: x => x[2]});
    const direction = [[1, 0], [-1, 0], [0, 1], [0, -1]];
    
    heap.enqueue([0, 0, grid[0][0]]);
    const visited = Array.from({ length: n }, () => Array(n).fill(false));
    visited[0][0] = true;
    let ans = 0;
  
    while (!heap.isEmpty()) {
        const { element: [x, y, time] } = heap.dequeue();
        ans = Math.max(ans, time);
        if (x === n - 1 && y === n - 1) return ans;
        
        for (const [dx, dy] of direction) {
            const nx = x + dx, ny = y + dy;
            if (nx >= 0 && nx < n && ny >= 0 && ny < n && !visited[nx][ny]) {
                visited[nx][ny] = true;
                heap.enqueue([nx, ny, grid[nx][ny]]);
            }
        }
    }
    return ans;
}
```

**Time Complexity:** O(N^2 log(N)), for the priority queue operations.

**Space Complexity:** O(N^2), for the visited grid and the heap.

### Binary Search with BFS

**Intuition:**

Binary search over the water level to find the minimum time at which we can swim from the top-left to the bottom-right. Verify the feasibility of swimming at a given water level using BFS. This approach uses BFS to check the reachability and binary search to minimize the said water level.

**Code:**

```javascript
function swimInWater(grid) {
    const n = grid.length;
    const direction = [[1, 0], [-1, 0], [0, 1], [0, -1]];

    function canSwim(waterLevel) {
        const visited = Array.from({ length: n }, () => Array(n).fill(false));
        const queue = [[0, 0]];
        visited[0][0] = true;
        
        while (queue.length > 0) {
            const [x, y] = queue.shift();
            if (x === n - 1 && y === n - 1) return true;
            
            for (let [dx, dy] of direction) {
                const nx = x + dx, ny = y + dy;
                if (
                    nx >= 0 && nx < n && ny >= 0 && ny < n &&
                    !visited[nx][ny] && grid[nx][ny] <= waterLevel
                ) {
                    visited[nx][ny] = true;
                    queue.push([nx, ny]);
                }
            }
        }
        return false;
    }
    
    let left = grid[0][0], right = n * n - 1;
    while (left < right) {
        const mid = Math.floor((left + right) / 2);
        if (canSwim(mid)) right = mid;
        else left = mid + 1;
    }
    return left;
}
```

**Time Complexity:** O(N^2 log(N)), where the log factor is due to binary search.

**Space Complexity:** O(N^2), for the BFS queue and visited array.

