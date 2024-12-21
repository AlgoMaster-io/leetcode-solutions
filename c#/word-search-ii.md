## [LeetCode 212: Word Search II](https://leetcode.com/problems/word-search-ii/)

### Approaches:
- [Approach 1: Backtracking with DFS](#approach-1-backtracking-with-dfs)
- [Approach 2: Trie with Backtracking](#approach-2-trie-with-backtracking)
  
---

### Approach 1: Backtracking with DFS

#### Intuition:
This problem involves searching for words in a 2D grid, which can be achieved using Depth-First Search (DFS) combined with backtracking. We explore all possible paths for each word, ensuring we do not revisit cells and only follow valid paths. This brute-force approach is straightforward but can be inefficient for larger grids and word lists.

#### Steps:
1. Iterate over each cell in the grid.
2. From each starting cell, attempt to form each word in the list using DFS:
   - Move in all four directions (up, down, left, right).
   - Keep track of visited cells to avoid revisiting.
   - Abandon paths that do not match the word.
3. If a full word is matched, add it to the result.

#### Time Complexity:
- **O(N * M * 4^L)**, where `N` is the number of rows, `M` is the number of columns, and `L` is the length of the longest word. Each cell can be explored in four directions, up to a maximum length `L`.

#### Space Complexity:
- **O(L)**: The stack space used for recursion can grow up to the length of the longest word.

```csharp
public class Solution 
{
    private List<string> result;
    private char[,] board;
    private bool[,] visited;
    private int rows;
    private int cols;
    
    public IList<string> FindWords(char[,] board, string[] words) 
    {
        result = new List<string>();
        this.board = board;
        rows = board.GetLength(0);
        cols = board.GetLength(1);
        visited = new bool[rows, cols];
        
        foreach (string word in words) 
        {
            for (int r = 0; r < rows; r++) 
            {
                for (int c = 0; c < cols; c++) 
                {
                    if (DFS(r, c, word, 0)) 
                    {
                        result.Add(word);
                        break;
                    }
                }
            }
        }
        return result;
    }
    
    private bool DFS(int r, int c, string word, int index) 
    {
        if (index == word.Length) 
        {
            return true; // Entire word is matched
        }
        
        if (r < 0 || r == rows || c < 0 || c == cols || visited[r, c] || board[r, c] != word[index]) 
        {
            return false; // Out of bounds or not matching
        }
        
        visited[r, c] = true; // Mark the cell as visited
        bool found = DFS(r + 1, c, word, index + 1) ||
                     DFS(r - 1, c, word, index + 1) ||
                     DFS(r, c + 1, word, index + 1) ||
                     DFS(r, c - 1, word, index + 1);
                     
        visited[r, c] = false; // Unmark the cell
        return found;
    }
}
```

---

### Approach 2: Trie with Backtracking

#### Intuition:
To optimize the search, we can use a Trie. A Trie structures the list of words such that common prefixes are stored only once. This reduces redundancy in search operations and helps in quicker pruning of paths that do not lead to complete words.

#### Steps:
1. Insert all words into a Trie.
2. For each cell on the board, search for words starting from that cell using DFS.
3. Use the Trie to immediately prune paths that cannot form a valid word prefix.
4. Mark words as found in the Trie to ensure duplicate entries are avoided.

#### Time Complexity:
- **O(N * M * L)**: Each cell is visited at most once in each direction, leveraging the Trie for efficient prefix checking.

#### Space Complexity:
- **O(W * L)**, where `W` is the number of words and `L` is the average length of the words, for the Trie storage.

```csharp
public class TrieNode 
{
    public TrieNode[] children = new TrieNode[26];
    public string word = null;
}

public class Solution 
{
    private List<string> result = new List<string>();
    
    public IList<string> FindWords(char[,] board, string[] words) 
    {
        TrieNode root = BuildTrie(words);
        int rows = board.GetLength(0);
        int cols = board.GetLength(1);
        
        for (int r = 0; r < rows; r++) 
        {
            for (int c = 0; c < cols; c++) 
            {
                DFS(board, r, c, root);
            }
        }
        
        return result;
    }
    
    private TrieNode BuildTrie(string[] words) 
    {
        TrieNode root = new TrieNode();
        
        foreach (string word in words) 
        {
            TrieNode node = root;
            
            foreach (char letter in word) 
            {
                int index = letter - 'a';
                if (node.children[index] == null) 
                {
                    node.children[index] = new TrieNode();
                }
                node = node.children[index];
            }
            node.word = word; // Mark end of word
        }
        
        return root;
    }
    
    private void DFS(char[,] board, int r, int c, TrieNode node) 
    {
        char letter = board[r, c];
        int index = letter - 'a';
        
        if (letter == '#' || node.children[index] == null) 
        {
            return; // Not a valid path
        }
        
        node = node.children[index];
        
        // Check if we found a complete word
        if (node.word != null) 
        {
            result.Add(node.word);
            node.word = null; // Avoid duplicate entries
        }
        
        board[r, c] = '#'; // Mark the cell as visited
        if (r > 0) DFS(board, r - 1, c, node);    // Explore upward
        if (r < board.GetLength(0) - 1) DFS(board, r + 1, c, node); // Explore downward
        if (c > 0) DFS(board, r, c - 1, node);    // Explore left
        if (c < board.GetLength(1) - 1) DFS(board, r, c + 1, node); // Explore right
        board[r, c] = letter; // Restore the cell
    }
}
```

This approach provides a significant performance improvement by using the Trie data structure, making the search for words much more efficient, especially in large grids or with lengthy lists of words.

