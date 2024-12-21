# [Leetcode 212: Word Search II](https://leetcode.com/problems/word-search-ii/)

## Approaches
- [Approach 1: Trie based Backtracking (Basic)](#approach-1-trie-based-backtracking-basic)
- [Approach 2: Optimized Trie based Backtracking (Optimal)](#approach-2-optimized-trie-based-backtracking-optimal)

## Approach 1: Trie based Backtracking (Basic)

### Intuition
The Word Search II problem can be efficiently solved using a Trie (prefix tree) and backtracking. The idea is to construct a Trie from the given list of words. Then, we perform DFS (backtracking) on each cell of the board to explore potential words. The Trie helps quickly verify if the current path can lead to a valid word.

### Detailed Steps
1. **Trie Construction**: Build a Trie out of the list of words. This allows us to quickly check prefixes and determine if a path in the board can lead to a valid word.
2. **Backtracking Function**: Implement a DFS function to explore each cell of the board. If a character matches the start of a Trie path, recursively explore its neighbors.
3. **Base Cases**: 
   - If the path corresponds to a word in the Trie, add it to the result list.
   - Stop exploration if the path does not match any Trie path.
4. **Visited Cells**: Use a visited set to prevent revisiting cells in the same path.

### Python Code
```python
class TrieNode:
    def __init__(self):
        self.children = {}
        self.isEndOfWord = False

def build_trie(words):
    root = TrieNode()
    for word in words:
        node = root
        for char in word:
            if char not in node.children:
                node.children[char] = TrieNode()
            node = node.children[char]
        node.isEndOfWord = True
    return root

def findWords(board, words):
    def backtrack(r, c, node, path):
        if node.isEndOfWord:
            found_words.add(path)
            
        # Mark this cell as visited
        temp, board[r][c] = board[r][c], '#'
        
        # Explore neighbors
        for dr, dc in [(0,1), (1,0), (0,-1), (-1,0)]:
            nr, nc = r + dr, c + dc
            if (0 <= nr < len(board) and 0 <= nc < len(board[0]) and
                board[nr][nc] in node.children):
                backtrack(nr, nc, node.children[board[nr][nc]], path + board[nr][nc])
        
        # Unmark this cell
        board[r][c] = temp

    root = build_trie(words)
    found_words = set()
    
    for row in range(len(board)):
        for col in range(len(board[0])):
            if board[row][col] in root.children:
                backtrack(row, col, root.children[board[row][col]], board[row][col])
    
    return list(found_words)
```

### Complexity Analysis
- **Time Complexity**: O(N * (4 * 3^(L-1)) + M * L), where N is the number of cells in the board, M is the number of words, and L is the maximum length of a word. For each call to DFS, we can explore at most 3 additional options excluding the direction we came from.
- **Space Complexity**: O(N + M * L), space for the Trie and the recursion stack in the worst case.

## Approach 2: Optimized Trie based Backtracking (Optimal)

### Intuition
This approach optimizes the previous solution by pruning the Trie as we explore the board. Whenever a word is found, we remove it from the Trie to potentially reduce DFS paths in future explorations.

### Detailed Steps
1. **Prune Trie**: Once a word is found, remove it from the Trie to prevent the same word from being added again and reduce DFS paths.
2. **Same DFS and Trie Building**: The DFS and Trie building remain mostly the same, except the additional step of pruning.

### Python Code
```python
def findWords(board, words):
    def backtrack(r, c, node, path):
        if node.isEndOfWord:
            found_words.add(path)
            node.isEndOfWord = False  # Avoid duplicates by marking as not end
        
        temp, board[r][c] = board[r][c], '#'
        
        for dr, dc in [(0,1), (1,0), (0,-1), (-1,0)]:
            nr, nc = r + dr, c + dc
            if (0 <= nr < len(board) and 0 <= nc < len(board[0]) and
                board[nr][nc] in node.children):
                backtrack(nr, nc, node.children[board[nr][nc]], path + board[nr][nc])
        
        board[r][c] = temp
        
        # Optimization: prune the Trie
        if not node.children:  # If current node has no children, prune it
            del node.parent[node.char]  

    root = build_trie(words)
    found_words = set()
    
    for row in range(len(board)):
        for col in range(len(board[0])):
            if board[row][col] in root.children:
                backtrack(row, col, root.children[board[row][col]], board[row][col])
    
    return list(found_words)
```

### Complexity Analysis
- **Time Complexity**: Same but practically faster than Approach 1 due to early termination and Trie pruning.
- **Space Complexity**: Same as Approach 1 due to additional nodes being removed during execution.

Both solutions effectively solve the problem, but the second approach with Trie pruning helps reduce unnecessary exploration, thereby optimizing the search in specific scenarios.

