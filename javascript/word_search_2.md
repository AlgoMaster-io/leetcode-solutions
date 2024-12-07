# 212. [Word Search II](https://leetcode.com/problems/word-search-ii/)

## Approach 1: Trie + Backtracking

### Solution
```javascript
// Time Complexity: O(n * m * 4^l), where n and m are the dimensions of the board, and l is the average word length
// Space Complexity: O(w * l), where w is the number of words and l is the average length of a word in the dictionary
class TrieNode {
    constructor() {
        this.children = new Map();
        this.word = null; // Store the word ending at this node
    }
}

var findWords = function(board, words) {
    // Build the Trie
    const root = new TrieNode();
    for (const word of words) {
        let current = root;
        for (const c of word) {
            if (!current.children.has(c)) {
                current.children.set(c, new TrieNode());
            }
            current = current.children.get(c);
        }
        current.word = word; // Mark the end of a word
    }

    const result = [];
    for (let i = 0; i < board.length; i++) {
        for (let j = 0; j < board[0].length; j++) {
            backtrack(board, i, j, root, result);
        }
    }

    return result;
};

function backtrack(board, row, col, node, result) {
    const letter = board[row][col];
    const current = node.children.get(letter);

    // If no Trie node exists, return
    if (!current) return;

    // Check if the current node contains a word
    if (current.word) {
        result.push(current.word);
        current.word = null; // Avoid duplicate entries
    }

    // Mark the current cell as visited
    board[row][col] = '#';

    // Explore the neighbors
    const rowOffsets = [-1, 1, 0, 0];
    const colOffsets = [0, 0, -1, 1];
    for (let i = 0; i < 4; i++) {
        const newRow = row + rowOffsets[i];
        const newCol = col + colOffsets[i];
        if (newRow >= 0 && newRow < board.length && newCol >= 0 && newCol < board[0].length) {
            backtrack(board, newRow, newCol, current, result);
        }
    }

    // Restore the cell value
    board[row][col] = letter;

    // Optimization: Remove the leaf Trie node
    if (current.children.size === 0) {
        node.children.delete(letter);
    }
}
```

