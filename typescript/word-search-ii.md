# [Leetcode 212: Word Search II](https://leetcode.com/problems/word-search-ii/)

## Approaches
- [Approach 1: Backtracking for each word](#approach-1-backtracking-for-each-word)
- [Approach 2: Trie + Backtracking](#approach-2-trie--backtracking)

## Approach 1: Backtracking for each word

**Intuition:**

In this approach, we attempt to find each word in the board individually starting from each cell, using backtracking. This is a straightforward method but could be inefficient for larger grids and word lists as it involves significant redundancy.

**Steps:**

1. For every word in the list:
   - Start from every cell in the grid.
   - Attempt to find the word using Depth First Search (DFS).
2. The DFS function will recursively search in four directions, marking cells as visited by altering their value with a special character (e.g., '#') temporarily.
3. If the end of the word is reached, it's found, add it to the results.
4. Backtrack by restoring the original cell values once done with the current path.

**Code:**

```typescript
function findWords(board: string[][], words: string[]): string[] {
    const result: string[] = [];

    // Helper function for backtracking
    function search(x: number, y: number, word: string, index: number): boolean {
        // Base cases
        if (index === word.length) return true; // Word completely found
        if (x < 0 || y < 0 || x >= board.length || y >= board[0].length) return false; // Out of bounds
        if (board[x][y] !== word[index]) return false; // Character does not match

        // Mark as visited
        const temp = board[x][y];
        board[x][y] = '#'; // Temporarily mark cell as visited

        // Explore in all four directions
        const found = search(x + 1, y, word, index + 1) ||
                      search(x - 1, y, word, index + 1) ||
                      search(x, y + 1, word, index + 1) ||
                      search(x, y - 1, word, index + 1);

        // Backtrack
        board[x][y] = temp; // Restore original value to backtrack

        return found;
    }

    for (const word of words) {
        for (let i = 0; i < board.length; i++) {
            for (let j = 0; j < board[0].length; j++) {
                if (search(i, j, word, 0)) {
                    result.push(word);
                    break; // Move to the next word after finding current
                }
            }
        }
    }

    return result;
}
```

**Time Complexity:** O(N * M * 4^L * W) where N and M are the board's dimensions, L is the length of the longest word, and W is the number of words.

**Space Complexity:** O(L) for the recursion stack, where L is the length of the word being searched for.

## Approach 2: Trie + Backtracking

**Intuition:**

This optimal solution uses a Trie to hold all words and reduces redundancy in searching for each word individually on the board. The Trie makes it efficient to explore common prefixes once and backtracking is well-suited for this recursive exploration.

**Steps:**

1. Build a Trie where each node represents a character and path leads to the end of a word.
2. Start traversing the board similarly as in Approach 1.
3. As you navigate through the board, also traverse through the Trie.
4. If entire path (word) is found in both Trie and board, it is valid and added to results.
5. Use the Trie to limit paths when a character or word in the board has no corresponding Trie path.

**Code:**

```typescript
class TrieNode {
    children: { [key: string]: TrieNode } = {};
    word: string | null = null; // Store complete word at the end node
}

function findWords(board: string[][], words: string[]): string[] {
    const root = new TrieNode();

    // Helper to build the Trie
    const buildTrie = (word: string) => {
        let node = root;
        for (const char of word) {
            if (!node.children[char]) {
                node.children[char] = new TrieNode();
            }
            node = node.children[char];
        }
        node.word = word; // Mark end of a word
    };

    for (const word of words) {
        buildTrie(word);
    }

    const result: string[] = [];

    // Backtracking with Trie
    function search(x: number, y: number, node: TrieNode) {
        if (node.word !== null) {
            result.push(node.word);
            node.word = null; // Avoid duplicates
        }

        if (x < 0 || y < 0 || x >= board.length || y >= board[0].length || !node.children[board[x][y]]) {
            return;
        }

        const char = board[x][y];
        const childNode = node.children[char];

        // Visit cell
        board[x][y] = '#';

        // Explore all directions
        search(x + 1, y, childNode);
        search(x - 1, y, childNode);
        search(x, y + 1, childNode);
        search(x, y - 1, childNode);

        // Backtracking
        board[x][y] = char; // Restore original state

        // Optimization: prune leaf node
        if (Object.keys(childNode.children).length === 0) {
            delete node.children[char];
        }
    }

    for (let i = 0; i < board.length; i++) {
        for (let j = 0; j < board[0].length; j++) {
            if (root.children[board[i][j]]) {
                search(i, j, root);
            }
        }
    }

    return result;
}
```

**Time Complexity:** O(N * M * 4^L) where N and M are the board's dimensions, and L is the maximum length of words.

**Space Complexity:** O(L * W) for the Trie storage, where L is the longest word's length, and W is the number of words. Recursion uses O(L) stack space.

