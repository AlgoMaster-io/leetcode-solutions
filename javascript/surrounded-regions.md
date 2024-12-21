# [Leetcode 130: Surrounded Regions](https://leetcode.com/problems/surrounded-regions/)

## Approaches
- [Approach 1: DFS from Boundary](#approach-1-dfs-from-boundary)
- [Approach 2: BFS from Boundary](#approach-2-bfs-from-boundary)
- [Approach 3: Union-Find](#approach-3-union-find)

## Approach 1: DFS from Boundary

### Intuition:
The idea is to find all 'O's that cannot be flipped to 'X' because they are connected to the boundary. We start DFS from every 'O' on the boundary and mark all connected 'O's as non-flippable by changing them to a temporary marker like 'T'. After processing, we flip all remaining 'O's to 'X', as these are surrounded, and change all 'T's back to 'O'.

### Algorithm:
1. Traverse the board and initiate a DFS from each 'O' found on the boundary.
2. Mark each 'O' found during DFS exploration as 'T'.
3. Iterate the entire board again, flipping all 'O's to 'X' (these are surrounded ones), and 'T's back to 'O'.

### Code:
```javascript
function solve(board) {
    if (!board || board.length === 0) return;
    const rows = board.length;
    const cols = board[0].length;
    
    // Helper function for DFS
    function dfs(r, c) {
        if (r < 0 || c < 0 || r >= rows || c >= cols || board[r][c] !== 'O') {
            return;
        }
        // Mark this cell as part of non-flippable region
        board[r][c] = 'T';
        // Explore in all four directions
        dfs(r + 1, c);
        dfs(r - 1, c);
        dfs(r, c + 1);
        dfs(r, c - 1);
    }
    
    // Start DFS from 'O's on the boundary
    for (let i = 0; i < rows; i++) {
        dfs(i, 0); // Left boundary
        dfs(i, cols - 1); // Right boundary
    }
    for (let j = 0; j < cols; j++) {
        dfs(0, j); // Top boundary
        dfs(rows - 1, j); // Bottom boundary
    }
    
    // Flip 'O' to 'X' and 'T' back to 'O'
    for (let i = 0; i < rows; i++) {
        for (let j = 0; j < cols; j++) {
            if (board[i][j] === 'O') {
                board[i][j] = 'X';
            } else if (board[i][j] === 'T') {
                board[i][j] = 'O';
            }
        }
    }
}
```

### Time Complexity:
- O(m * n), where m is the number of rows and n is the number of columns. We potentially visit each cell multiple times.

### Space Complexity:
- O(m * n) in the worst case due to the recursion stack in DFS. However, in practice, it's more efficient than the worst case.

## Approach 2: BFS from Boundary

### Intuition:
Instead of DFS, we can also use BFS to mark the non-flippable 'O's. The logic remains the same, only the traversal mechanism changes.

### Code:
```javascript
function solve(board) {
    if (!board || board.length === 0) return;
    const rows = board.length;
    const cols = board[0].length;
    
    // Helper function for BFS
    function bfs(r, c) {
        const queue = [];
        queue.push([r, c]);
        board[r][c] = 'T';
        
        const directions = [[1, 0], [-1, 0], [0, 1], [0, -1]];
        while (queue.length > 0) {
            const [currR, currC] = queue.shift();
            for (const [dR, dC] of directions) {
                const newR = currR + dR;
                const newC = currC + dC;
                if (newR >= 0 && newC >= 0 && newR < rows && newC < cols && board[newR][newC] === 'O') {
                    queue.push([newR, newC]);
                    board[newR][newC] = 'T';
                }
            }
        }
    }
    
    for (let i = 0; i < rows; i++) {
        if (board[i][0] === 'O') bfs(i, 0);
        if (board[i][cols - 1] === 'O') bfs(i, cols - 1);
    }
    for (let j = 0; j < cols; j++) {
        if (board[0][j] === 'O') bfs(0, j);
        if (board[rows - 1][j] === 'O') bfs(rows - 1, j);
    }
    
    // Flip 'O' to 'X' and 'T' back to 'O'
    for (let i = 0; i < rows; i++) {
        for (let j = 0; j < cols; j++) {
            if (board[i][j] === 'O') {
                board[i][j] = 'X';
            } else if (board[i][j] === 'T') {
                board[i][j] = 'O';
            }
        }
    }
}
```

### Time Complexity:
- O(m * n), where m is the number of rows and n is the number of columns.

### Space Complexity:
- O(min(m, n)) which is the size of the queue.

## Approach 3: Union-Find

### Intuition:
Leverage the Union-Find data structure to connect all 'O's on the boundary and attach them to a virtual dummy node. The ones connected to this dummy node are safe and cannot be flipped. All other 'O's can be flipped to 'X'.

### Code:
```javascript
class UnionFind {
    constructor(size) {
        this.parent = Array(size);
        this.rank = Array(size).fill(1);
        for (let i = 0; i < size; i++) {
            this.parent[i] = i;
        }
    }

    find(x) {
        if (this.parent[x] !== x) {
            this.parent[x] = this.find(this.parent[x]);
        }
        return this.parent[x];
    }

    union(x, y) {
        const rootX = this.find(x);
        const rootY = this.find(y);

        if (rootX !== rootY) {
            if (this.rank[rootX] > this.rank[rootY]) {
                this.parent[rootY] = rootX;
            } else if (this.rank[rootX] < this.rank[rootY]) {
                this.parent[rootX] = rootY;
            } else {
                this.parent[rootY] = rootX;
                this.rank[rootX]++;
            }
        }
    }
}

function solve(board) {
    if (!board || board.length === 0) return;
    const rows = board.length;
    const cols = board[0].length;
    const dummyNode = rows * cols;

    const uf = new UnionFind(rows * cols + 1);

    // Union boundary 'O's with dummy node
    for (let i = 0; i < rows; i++) {
        for (let j = 0; j < cols; j++) {
            if (board[i][j] === 'O') {
                if (i === 0 || j === 0 || i === rows - 1 || j === cols - 1) {
                    uf.union(i * cols + j, dummyNode);
                } else {
                    if (board[i - 1][j] === 'O') uf.union(i * cols + j, (i - 1) * cols + j);
                    if (board[i + 1][j] === 'O') uf.union(i * cols + j, (i + 1) * cols + j);
                    if (board[i][j - 1] === 'O') uf.union(i * cols + j, i * cols + j - 1);
                    if (board[i][j + 1] === 'O') uf.union(i * cols + j, i * cols + j + 1);
                }
            }
        }
    }

    // Flip cells not connected to dummy node
    for (let i = 0; i < rows; i++) {
        for (let j = 0; j < cols; j++) {
            if (board[i][j] === 'O' && uf.find(i * cols + j) !== uf.find(dummyNode)) {
                board[i][j] = 'X';
            }
        }
    }
}
```

### Time Complexity:
- O(m * n * α(m * n)), where α is the Inverse Ackermann function, which is very slow-growing and almost constant for practical purposes.

### Space Complexity:
- O(m * n) for the Union-Find data structure.

