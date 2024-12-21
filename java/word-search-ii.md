## [Leetcode 212: Word Search II](https://leetcode.com/problems/word-search-ii/)

**Approaches:**

1. [Brute Force Approach](#brute-force-approach)  
2. [Backtracking with Trie](#backtracking-with-trie)

### 1. Brute Force Approach

**Intuition:**

In the brute force approach, we iterate over each word and place it character by character on the board, starting from every potential starting point. This involves a depth-first search (DFS) from each cell for each word and checking all four possible directions. This naive method helps understand the base working but lacks efficiency.

**Java Code:**

```java
class Solution {
    public List<String> findWords(char[][] board, String[] words) {
        Set<String> result = new HashSet<>();
        for (String word : words) {
            if (exist(board, word)) {
                result.add(word);
            }
        }
        return new ArrayList<>(result);
    }

    private boolean exist(char[][] board, String word) {
        int rows = board.length, cols = board[0].length;
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                if (dfs(board, word, i, j, 0)) {
                    return true;
                }
            }
        }
        return false;
    }

    private boolean dfs(char[][] board, String word, int i, int j, int index) {
        if (index == word.length()) {
            return true;
        }
        if (i < 0 || i >= board.length || j < 0 || j >= board[0].length || board[i][j] != word.charAt(index)) {
            return false;
        }
        char temp = board[i][j];
        board[i][j] = '#'; // mark as visited
        boolean found = dfs(board, word, i + 1, j, index + 1) ||
                        dfs(board, word, i - 1, j, index + 1) ||
                        dfs(board, word, i, j + 1, index + 1) ||
                        dfs(board, word, i, j - 1, index + 1);
        board[i][j] = temp; // backtrack
        return found;
    }
}
```

**Time Complexity:** \(O(N \times M \times 4^L)\), where \(N \times M\) is the size of the board, and \(L\) is the length of the longest word.  
**Space Complexity:** \(O(L)\), recursion depth for backtracking.

### 2. Backtracking with Trie

**Intuition:**

To optimize, we use a Trie data structure for efficient prefix checking. Trie allows us to minimize unnecessary DFS calls by terminating branches early if a prefix leads to no valid word. For each starting point on the board, we search using the trie to construct words, reducing redundant operations considerably compared to checking each word independently.

**Java Code:**

```java
class Solution {
    public List<String> findWords(char[][] board, String[] words) {
        Set<String> result = new HashSet<>();
        TrieNode root = buildTrie(words);
        int rows = board.length, cols = board[0].length;
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                dfs(board, i, j, root, result);
            }
        }
        return new ArrayList<>(result);
    }

    private void dfs(char[][] board, int i, int j, TrieNode node, Set<String> result) {
        char c = board[i][j];
        if (c == '#' || node.next[c - 'a'] == null) return;
        node = node.next[c - 'a'];
        if (node.word != null) { // found a word
            result.add(node.word);
            node.word = null; // avoid duplicates
        }
        board[i][j] = '#'; // mark as visited
        if (i > 0) dfs(board, i - 1, j, node, result);
        if (i < board.length - 1) dfs(board, i + 1, j, node, result);
        if (j > 0) dfs(board, i, j - 1, node, result);
        if (j < board[0].length - 1) dfs(board, i, j + 1, node, result);
        board[i][j] = c; // restore state
    }

    private TrieNode buildTrie(String[] words) {
        TrieNode root = new TrieNode();
        for (String word : words) {
            TrieNode node = root;
            for (char c : word.toCharArray()) {
                if (node.next[c - 'a'] == null) {
                    node.next[c - 'a'] = new TrieNode();
                }
                node = node.next[c - 'a'];
            }
            node.word = word;
        }
        return root;
    }

    class TrieNode {
        TrieNode[] next = new TrieNode[26];
        String word;
    }
}
```

**Time Complexity:** \(O(N \times M \times 4^L)\), but often faster in practice due to trie pruning.  
**Space Complexity:** \(O(N \times M + W \times L)\), where \(W\) is the number of words and \(L\) is the average word length (Trie size).

