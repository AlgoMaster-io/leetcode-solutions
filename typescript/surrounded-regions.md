# [Leetcode 130: Surrounded Regions](https://leetcode.com/problems/surrounded-regions/)

## Table of Contents
- [Approach 1: Depth-First Search (DFS)](#approach-1-depth-first-search-dfs)
- [Approach 2: Breadth-First Search (BFS)](#approach-2-breadth-first-search-bfs)
- [Approach 3: Union-Find](#approach-3-union-find)

## Approach 1: Depth-First Search (DFS)

### Intuition
The idea is to protect the 'O's that are connected to the boundaries by marking them as non-flippable. We can start a DFS from each boundary 'O', marking all 'O's connected to it as a special character (like '#'). After processing the entire board, we'll flip all remaining 'O's (which are surrounded by 'X's) to 'X' and change '#' back to 'O'.

### Algorithm
1. Iterate over the border of the grid (first row, last row, first column, last column).
2. Start a DFS for each 'O' found on the border and mark it and all connected 'O's as '#'.
3. After the DFS is complete for the entire grid, scan through every cell and:
   - Convert all 'O's to 'X', as they are surrounded.
   - Convert all '#' back to 'O', as they are connected to the border and not surrounded.

### Code
```typescript
function solve(board: string[][]): void {
    if (!board || board.length === 0 || board[0].length === 0) return;

    const rows = board.length;
    const cols = board[0].length;

    // Perform DFS from all 'O's on the border
    for (let r = 0; r < rows; r++) {
        if (board[r][0] === 'O') dfs(board, r, 0);
        if (board[r][cols - 1] === 'O') dfs(board, r, cols - 1);
    }
    for (let c = 0; c < cols; c++) {
        if (board[0][c] === 'O') dfs(board, 0, c);
        if (board[rows - 1][c] === 'O') dfs(board, rows - 1, c);
    }

    // Transform the board as per the rules
    for (let r = 0; r < rows; r++) {
        for (let c = 0; c < cols; c++) {
            if (board[r][c] === 'O') {
                board[r][c] = 'X';  // Surrounded regions
            } else if (board[r][c] === '#') {
                board[r][c] = 'O';  // Non-surrounded regions
            }
        }
    }
}

// Helper DFS function
function dfs(board: string[][], r: number, c: number): void {
    const rows = board.length;
    const cols = board[0].length;

    if (r < 0 || r >= rows || c < 0 || c >= cols || board[r][c] !== 'O') {
        return;
    }

    // Mark the cell as part of the non-surrounded region
    board[r][c] = '#';

    // Explore the neighboring cells
    dfs(board, r - 1, c); // Up
    dfs(board, r + 1, c); // Down
    dfs(board, r, c - 1); // Left
    dfs(board, r, c + 1); // Right
}
```

### Complexity
- Time Complexity: \(O(m \times n)\) where \(m\) is the number of rows and \(n\) is the number of columns. We potentially visit every cell.
- Space Complexity: \(O(m \times n)\) for the recursion stack.

## Approach 2: Breadth-First Search (BFS)

### Intuition
Similar to DFS, we can use a queue to perform a breadth-first search starting from all boundary 'O's, marking all connected 'O's as non-flippable by using a special character.

### Algorithm
1. Use a queue to implement BFS from all 'O's on the border.
2. As you dequeue, check its neighboring cells and enqueue them if they are 'O', marking them as '#'.
3. After processing, convert the entire board as in approach 1.

### Code
```typescript
function solve(board: string[][]): void {
    if (!board || board.length === 0 || board[0].length === 0) return;

    const rows = board.length;
    const cols = board[0].length;

    // Use a queue to perform BFS
    const queue: [number, number][] = [];

    // Add all border 'O's to the queue
    for (let r = 0; r < rows; r++) {
        if (board[r][0] === 'O') queue.push([r, 0]);
        if (board[r][cols - 1] === 'O') queue.push([r, cols - 1]);
    }
    for (let c = 0; c < cols; c++) {
        if (board[0][c] === 'O') queue.push([0, c]);
        if (board[rows - 1][c] === 'O') queue.push([rows - 1, c]);
    }

    // BFS to mark non-surrounded regions
    while (queue.length > 0) {
        const [r, c] = queue.shift()!;
        if (r < 0 || r >= rows || c < 0 || c >= cols || board[r][c] !== 'O') continue;

        // Mark the cell as part of the non-surrounded region
        board[r][c] = '#';

        // Add neighboring cells to the queue
        queue.push([r - 1, c], [r + 1, c], [r, c - 1], [r, c + 1]);
    }

    // Transform the board as per the rules
    for (let r = 0; r < rows; r++) {
        for (let c = 0; c < cols; c++) {
            if (board[r][c] === 'O') {
                board[r][c] = 'X';  // Surrounded regions
            } else if (board[r][c] === '#') {
                board[r][c] = 'O';  // Non-surrounded regions
            }
        }
    }
}
```

### Complexity
- Time Complexity: \(O(m \times n)\), as every cell is potentially visited.
- Space Complexity: \(O(m \times n)\) for the queue.

## Approach 3: Union-Find

### Intuition
Using the union-find data structure, we can model the connectivity of 'O's, linking each boundary-connected 'O' to a virtual "boundary" node. After processing the entire board, we can determine which 'O's should be flipped by checking if they belong to the same set as the "boundary" node.

### Algorithm
1. Initialize union-find with an extra virtual node representing the boundary.
2. Iterate over the board, and unite 'O's with their neighboring 'O's if they are also part of boundary-connected components.
3. In the final pass, flip 'O's not connected to the boundary node.

### Code
```typescript
class UnionFind {
    parent: number[];
    rank: number[];

    constructor(size: number) {
        this.parent = Array.from({ length: size }, (_, i) => i);
        this.rank = Array(size).fill(0);
    }

    find(x: number): number {
        if (this.parent[x] !== x) {
            this.parent[x] = this.find(this.parent[x]); // Path compression
        }
        return this.parent[x];
    }

    union(x: number, y: number): void {
        const rootX = this.find(x);
        const rootY = this.find(y);
        if (rootX !== rootY) {
            if (this.rank[rootX] > this.rank[rootY]) {
                this.parent[rootY] = rootX;
            } else if (this.rank[rootX] < this.rank[rootY]) {
                this.parent[rootX] = rootY;
            } else {
                this.parent[rootY] = rootX;
                this.rank[rootX] += 1;
            }
        }
    }
}

function solve(board: string[][]): void {
    if (!board || board.length === 0 || board[0].length === 0) return;

    const rows = board.length;
    const cols = board[0].length;
    const uf = new UnionFind(rows * cols + 1);
    const boundaryNode = rows * cols;

    // Link all border 'O's to the boundary node
    for (let r = 0; r < rows; r++) {
        for (let c = 0; c < cols; c++) {
            if (board[r][c] === 'O') {
                const index = r * cols + c;
                if (r === 0 || c === 0 || r === rows - 1 || c === cols - 1) {
                    uf.union(index, boundaryNode);
                }
                // Union with neighboring cells
                if (r > 0 && board[r - 1][c] === 'O') uf.union(index, (r - 1) * cols + c);
                if (c > 0 && board[r][c - 1] === 'O') uf.union(index, r * cols + (c - 1));
            }
        }
    }

    // Process the board to flip regions
    for (let r = 0; r < rows; r++) {
        for (let c = 0; c < cols; c++) {
            if (board[r][c] === 'O') {
                const index = r * cols + c;
                if (uf.find(index) !== uf.find(boundaryNode)) {
                    board[r][c] = 'X';
                }
            }
        }
    }
}
```

### Complexity
- Time Complexity: \(O(m \times n \cdot \alpha(m \times n))\), where \(\alpha\) is the inverse Ackermann function, nearly constant.
- Space Complexity: \(O(m \times n)\) for the union-find structure.

This concludes a step-by-step breakdown of various methods to solve the "Surrounded Regions" problem using different approaches, along with TypeScript code implementations and complexity analysis.

