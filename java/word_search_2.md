# 212. [Word Search II](https://leetcode.com/problems/word-search-ii/)

## Approach 1: Trie + Backtracking

### Solution
```java
// Time Complexity: O(n * m * 4^l), where n and m are the dimensions of the board, and l is the average word length
// Space Complexity: O(w * l), where w is the number of words and l is the average length of a word in the dictionary
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;

class TrieNode {
    HashMap<Character, TrieNode> children = new HashMap<>();
    String word = null; // Store the word ending at this node
}

public class Solution {
    public List<String> findWords(char[][] board, String[] words) {
        // Build the Trie
        TrieNode root = new TrieNode();
        for (String word : words) {
            TrieNode current = root;
            for (char c : word.toCharArray()) {
                current.children.putIfAbsent(c, new TrieNode());
                current = current.children.get(c);
            }
            current.word = word; // Mark the end of a word
        }

        List<String> result = new ArrayList<>();
        for (int i = 0; i < board.length; i++) {
            for (int j = 0; j < board[0].length; j++) {
                backtrack(board, i, j, root, result);
            }
        }

        return result;
    }

    private void backtrack(char[][] board, int row, int col, TrieNode node, List<String> result) {
        char letter = board[row][col];
        TrieNode current = node.children.get(letter);

        // If no Trie node exists, return
        if (current == null) return;

        // Check if the current node contains a word
        if (current.word != null) {
            result.add(current.word);
            current.word = null; // Avoid duplicate entries
        }

        // Mark the current cell as visited
        board[row][col] = '#';

        // Explore the neighbors
        int[] rowOffsets = {-1, 1, 0, 0};
        int[] colOffsets = {0, 0, -1, 1};
        for (int i = 0; i < 4; i++) {
            int newRow = row + rowOffsets[i];
            int newCol = col + colOffsets[i];
            if (newRow >= 0 && newRow < board.length && newCol >= 0 && newCol < board[0].length) {
                backtrack(board, newRow, newCol, current, result);
            }
        }

        // Restore the cell value
        board[row][col] = letter;

        // Optimization: Remove the leaf Trie node
        if (current.children.isEmpty()) {
            node.children.remove(letter);
        }
    }
}
```