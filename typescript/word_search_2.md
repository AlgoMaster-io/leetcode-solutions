# 212. [Word Search II](https://leetcode.com/problems/word-search-ii/)

## Approach 1: Trie + Backtracking

### Solution
typescript
```typescript
// Time Complexity: O(n * m * 4^l), where n and m are the dimensions of the board, and l is the average word length
// Space Complexity: O(w * l), where w is the number of words and l is the average length of a word in the dictionary

class TrieNode {
    children: Map<string, TrieNode> = new Map();
    word: string | null = null; // Store the word ending at this node
}

class Solution {
    findWords(board: string[][], words: string[]): string[] {
        // Build the Trie
        let root = new TrieNode();
        for (let word of words) {
            let current = root;
            for (let c of word) {
                if (!current.children.has(c)) {
                    current.children.set(c, new TrieNode());
                }
                current = current.children.get(c)!;
            }
            current.word = word; // Mark the end of a word
        }

        let result: string[] = [];
        for (let i = 0; i < board.length; i++) {
            for (let j = 0; j < board[0].length; j++) {
                this.backtrack(board, i, j, root, result);
            }
        }

        return result;
    }

    private backtrack(board: string[][], row: number, col: number, node: TrieNode, result: string[]): void {
        let letter = board[row][col];
        let current = node.children.get(letter);

        // If no Trie node exists, return
        if (current == null) return;

        // Check if the current node contains a word
        if (current.word !== null) {
            result.push(current.word);
            current.word = null; // Avoid duplicate entries
        }

        // Mark the current cell as visited
        board[row][col] = '#';

        // Explore the neighbors
        let rowOffsets = [-1, 1, 0, 0];
        let colOffsets = [0, 0, -1, 1];
        for (let i = 0; i < 4; i++) {
            let newRow = row + rowOffsets[i];
            let newCol = col + colOffsets[i];
            if (newRow >= 0 && newRow < board.length && newCol >= 0 && newCol < board[0].length) {
                this.backtrack(board, newRow, newCol, current, result);
            }
        }

        // Restore the cell value
        board[row][col] = letter;

        // Optimization: Remove the leaf Trie node
        if (current.children.size === 0) {
            node.children.delete(letter);
        }
    }
}
```

