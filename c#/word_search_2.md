# 212. [Word Search II](https://leetcode.com/problems/word-search-ii/)

## Approach 1: Trie + Backtracking

### Solution
```csharp
// Time Complexity: O(n * m * 4^l), where n and m are the dimensions of the board, and l is the average word length
// Space Complexity: O(w * l), where w is the number of words and l is the average length of a word in the dictionary
using System.Collections.Generic;

class TrieNode {
    public Dictionary<char, TrieNode> Children = new Dictionary<char, TrieNode>();
    public string Word = null; // Store the word ending at this node
}

public class Solution {
    public IList<string> FindWords(char[][] board, string[] words) {
        // Build the Trie
        TrieNode root = new TrieNode();
        foreach (string word in words) {
            TrieNode current = root;
            foreach (char c in word) {
                if (!current.Children.ContainsKey(c)) {
                    current.Children[c] = new TrieNode();
                }
                current = current.Children[c];
            }
            current.Word = word; // Mark the end of a word
        }

        List<string> result = new List<string>();
        for (int i = 0; i < board.Length; i++) {
            for (int j = 0; j < board[0].Length; j++) {
                Backtrack(board, i, j, root, result);
            }
        }

        return result;
    }

    private void Backtrack(char[][] board, int row, int col, TrieNode node, List<string> result) {
        char letter = board[row][col];
        TrieNode current;
        if (!node.Children.TryGetValue(letter, out current)) return;

        // Check if the current node contains a word
        if (current.Word != null) {
            result.Add(current.Word);
            current.Word = null; // Avoid duplicate entries
        }

        // Mark the current cell as visited
        board[row][col] = '#';

        // Explore the neighbors
        int[] rowOffsets = {-1, 1, 0, 0};
        int[] colOffsets = {0, 0, -1, 1};
        for (int i = 0; i < 4; i++) {
            int newRow = row + rowOffsets[i];
            int newCol = col + colOffsets[i];
            if (newRow >= 0 && newRow < board.Length && newCol >= 0 && newCol < board[0].Length) {
                Backtrack(board, newRow, newCol, current, result);
            }
        }

        // Restore the cell value
        board[row][col] = letter;

        // Optimization: Remove the leaf Trie node
        if (current.Children.Count == 0) {
            node.Children.Remove(letter);
        }
    }
}
```

