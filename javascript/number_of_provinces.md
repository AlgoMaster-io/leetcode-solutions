# 547. [Number of Provinces](https://leetcode.com/problems/number-of-provinces/)

## Approach 1: Depth-First Search (DFS)

### Solution
```javascript
// Time Complexity: O(n^2)
// Space Complexity: O(n)
var findCircleNum = function(isConnected) {
    const n = isConnected.length;
    const visited = new Array(n).fill(false);
    let provinces = 0;

    const dfs = (i) => {
        visited[i] = true;
        for (let j = 0; j < n; j++) {
            if (isConnected[i][j] === 1 && !visited[j]) {
                dfs(j);
            }
        }
    };

    for (let i = 0; i < n; i++) {
        if (!visited[i]) {
            dfs(i);
            provinces++;
        }
    }

    return provinces;
};
```

## Approach 2: Breadth-First Search (BFS)

### Solution
```javascript
// Time Complexity: O(n^2)
// Space Complexity: O(n)
var findCircleNum = function(isConnected) {
    const n = isConnected.length;
    const visited = new Array(n).fill(false);
    let provinces = 0;

    const bfs = (i) => {
        const queue = [];
        queue.push(i);
        visited[i] = true;

        while (queue.length > 0) {
            const city = queue.shift();
            for (let j = 0; j < n; j++) {
                if (isConnected[city][j] === 1 && !visited[j]) {
                    queue.push(j);
                    visited[j] = true;
                }
            }
        }
    };

    for (let i = 0; i < n; i++) {
        if (!visited[i]) {
            bfs(i);
            provinces++;
        }
    }

    return provinces;
};
```

## Approach 3: Union-Find

### Solution
```javascript
// Time Complexity: O(n^2 * α(n))
// Space Complexity: O(n)
class UnionFind {
    constructor(n) {
        this.parent = new Array(n).fill(0).map((_, index) => index);
        this.count = n; // Number of provinces
    }

    find(x) {
        if (this.parent[x] !== x) {
            this.parent[x] = this.find(this.parent[x]); // Path compression
        }
        return this.parent[x];
    }

    union(x, y) {
        const rootX = this.find(x);
        const rootY = this.find(y);
        if (rootX !== rootY) {
            this.parent[rootX] = rootY; // Union by root
            this.count--;
        }
    }

    getCount() {
        return this.count;
    }
}

var findCircleNum = function(isConnected) {
    const n = isConnected.length;
    const uf = new UnionFind(n);

    for (let i = 0; i < n; i++) {
        for (let j = 0; j < n; j++) {
            if (isConnected[i][j] === 1) {
                uf.union(i, j);
            }
        }
    }

    return uf.getCount();
};
```

