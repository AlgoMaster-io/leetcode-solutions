# [Leetcode 212: Word Search II](https://leetcode.com/problems/word-search-ii/)

## Approaches

1. [Basic DFS Approach](#basic-dfs-approach)
2. [Optimized Trie-based DFS](#optimized-trie-based-dfs)

### Basic DFS Approach

#### Intuition:
The problem can be initially approached using Depth First Search (DFS) for each character in the grid, trying to form each word in the given list. If a path is found that forms one of the words, the word is added to the result set.

#### Steps:
1. Iterate over each cell of the grid.
2. Perform DFS searching for each word starting from the current cell.
3. Maintain a visited matrix to avoid revisiting cells during a single DFS.
4. If during a DFS, a word is found, add it to the result list.
5. Repeat for all words.

```javascript
var findWords = function(board, words) {
    const result = new Set();
    const rows = board.length;
    const cols = board[0].length;

    // Helper method for DFS
    function dfs(row, col, node, path) {
        // If indices are out of bounds or the cell is already visited, return
        if (row < 0 || col < 0 || row >= rows || col >= cols || board[row][col] === '*') {
            return;
        }
        
        const char = board[row][col];
        // Include current cell in the path
        path += char;
        // If the path is not a prefix of any word, stop the search
        if (!words.some(word => word.startsWith(path))) {
            return;
        }

        // Check if current path is in the words list and add it to result
        if (words.includes(path)) {
            result.add(path);
        }

        // Temporarily mark the cell as visited
        board[row][col] = '*';

        // Explore the neighbors in the four possible directions
        dfs(row + 1, col, node, path);
        dfs(row - 1, col, node, path);
        dfs(row, col + 1, node, path);
        dfs(row, col - 1, node, path);

        // Unmark the cell (backtrack)
        board[row][col] = char;
    }

    for (let row = 0; row < rows; row++) {
        for (let col = 0; col < cols; col++) {
            dfs(row, col, {}, '');
        }
    }

    return Array.from(result);
};
```

**Time Complexity**: O(N * (4^L)), where N is the number of cells in the board and L is the length of the words.  
**Space Complexity**: O(L), where L is the length of the words, required for recursion stack in DFS.

### Optimized Trie-based DFS

#### Intuition:
To efficiently match prefixes, we use a Trie to store the list of words. Instead of searching for each word separately, we use the Trie to guide the DFS, allowing us to quickly skip non-promising paths.

#### Steps:
1. Build a Trie structure from the given list of words.
2. Iterate over each cell of the grid.
3. For each cell, initiate a DFS using the Trie, checking nodes for matching prefixes.
4. If a node represents the end of a word, add it to the result list.
5. Use a visited matrix to prevent revisiting and efficiently backtrack when needed.

```javascript
class TrieNode {
    constructor() {
        this.children = {};
        this.isEndOfWord = false;
    }
}

class Trie {
    constructor() {
        this.root = new TrieNode();
    }

    insert(word) {
        let node = this.root;
        for (const char of word) {
            if (!node.children[char]) {
                node.children[char] = new TrieNode();
            }
            node = node.children[char];
        }
        node.isEndOfWord = true;
    }
}

var findWords = function(board, words) {
    const result = new Set();
    const trie = new Trie();
    for (const word of words) trie.insert(word);
    const rows = board.length;
    const cols = board[0].length;

    function dfs(row, col, node, path) {
        // If indices are out of bounds or the cell is already visited, return
        if (row < 0 || col < 0 || row >= rows || col >= cols || board[row][col] === '*') {
            return;
        }

        const char = board[row][col];
        const nextNode = node.children[char];
        if (!nextNode) {
            return;
        }

        path += char;
        if (nextNode.isEndOfWord) {
            result.add(path);
        }

        board[row][col] = '*';

        dfs(row + 1, col, nextNode, path);
        dfs(row - 1, col, nextNode, path);
        dfs(row, col + 1, nextNode, path);
        dfs(row, col - 1, nextNode, path);

        board[row][col] = char;
    }

    for (let row = 0; row < rows; row++) {
        for (let col = 0; col < cols; col++) {
            dfs(row, col, trie.root, '');
        }
    }

    return Array.from(result);
};
```

**Time Complexity**: O(N * 4^L), but much faster due to Trie skipping non-promising for paths, where N is the number of cells in the board and L is the maximum length of any word.  
**Space Complexity**: O(K), where K is the total length of all words combined stored in the Trie.

