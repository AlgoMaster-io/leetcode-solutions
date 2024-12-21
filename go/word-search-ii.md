# [Leetcode 212: Word Search II](https://leetcode.com/problems/word-search-ii/)

## Approaches
1. [Backtracking with Pruning](#backtracking-with-pruning)
2. [Backtracking with Trie](#backtracking-with-trie)

### Backtracking with Pruning

**Intuition:**

The problem involves searching for given words on a 2D grid, which can be approached with depth-first search (DFS), essentially backtracking. For each word, we could start a DFS from every cell, explore all possible word formations, and check for a match. However, this approach is highly inefficient without optimizations.

To improve performance, we can:
1. Start the DFS only from grid positions matching the first character of the word.
2. Prune paths early if they already exceed the word in question.
3. Avoid revisiting the same cell in one word formation attempt.

This approach gives an efficient solution for small word sets but grows exponentially with input size.

```go
func findWords(board [][]byte, words []string) []string {
    var result []string
    for _, word := range words {
        if exists(board, word) {
            result = append(result, word)
        }
    }
    return result
}

func exists(board [][]byte, word string) bool {
    for i := 0; i < len(board); i++ {
        for j := 0; j < len(board[0]); j++ {
            if backtrack(board, word, i, j, 0) {
                return true
            }
        }
    }
    return false
}

func backtrack(board [][]byte, word string, i, j, index int) bool {
    // Termination conditions: check boundaries and match current character
    if index >= len(word) { return true }
    if i < 0 || i >= len(board) || j < 0 || j >= len(board[0]) || board[i][j] != word[index] {
        return false
    }
    
    // Mark current cell as visited by choosing a safe marker, such as modifying the visited cell to a placeholder
    tmp := board[i][j]
    board[i][j] = '#'

    // Explore the neighbors in the grid
    found := backtrack(board, word, i+1, j, index+1) ||  // Down
             backtrack(board, word, i-1, j, index+1) ||  // Up
             backtrack(board, word, i, j+1, index+1) ||  // Right
             backtrack(board, word, i, j-1, index+1)     // Left

    // Revert the current cell mark for further explorations
    board[i][j] = tmp

    return found
}
```
- **Time Complexity:** O(N*M*4^L), where N and M are the dimensions of the board, and L is the length of the word. For each cell, there are 4 potential directions to explore.
- **Space Complexity:** O(L), the maximum depth of the recursion tree caused by the path length.

### Backtracking with Trie

**Intuition:**

To reduce repeated work, we use a Trie to store all words, allowing us to branch out only along valid word paths. This helps in skipping the search down unfruitful paths early, as the Trie will prevent unnecessary explorations.

1. Build a Trie from the words, marking the end of each word.
2. Backtrack from each cell, using Trie nodes to guide our search and avoid redundant checks.

The Trie structure allows efficient prefix checking and contributes greatly to reducing our search space.

```go
type TrieNode struct {
    children map[rune]*TrieNode
    word     string // stores the complete word when this node marks its end
}

func findWords(board [][]byte, words []string) []string {
    root := &TrieNode{children: make(map[rune]*TrieNode)}
    // Build the Trie from the word list
    for _, word := range words {
        node := root
        for _, ch := range word {
            if node.children[ch] == nil {
                node.children[ch] = &TrieNode{children: make(map[rune]*TrieNode)}
            }
            node = node.children[ch]
        }
        node.word = word
    }

    var result []string
    for i := 0; i < len(board); i++ {
        for j := 0; j < len(board[0]); j++ {
            backtrackWithTrie(board, i, j, root, &result)
        }
    }
    return result
}

func backtrackWithTrie(board [][]byte, i, j int, parent *TrieNode, result *[]string) {
    char := rune(board[i][j])
    // If no letter in the current trie path, we terminate this branch
    currNode, ok := parent.children[char]
    if !ok {
        return
    }

    // Found a word at the current trie level
    if currNode.word != "" {
        *result = append(*result, currNode.word)
        // Avoid duplicates in result list by setting back to empty
        currNode.word = ""
    }

    // Visit the cell
    board[i][j] = '#'
    
    // Explore neighbors in the grid
    if i > 0 { 
        backtrackWithTrie(board, i-1, j, currNode, result) 
    }
    if j > 0 { 
        backtrackWithTrie(board, i, j-1, currNode, result) 
    }
    if i < len(board)-1 { 
        backtrackWithTrie(board, i+1, j, currNode, result) 
    }
    if j < len(board[0])-1 { 
        backtrackWithTrie(board, i, j+1, currNode, result) 
    }
    
    // Restore original character
    board[i][j] = byte(char)

    // Optimization: prune the trie nodes if it's not needed anymore
    if len(currNode.children) == 0 {
        delete(parent.children, char)
    }
}
```
- **Time Complexity:** O(N * M * 4^L), similar to basic backtracking, but typically faster in practice due to pruning with Trie.
- **Space Complexity:** O(Trie + L), where Trie is the space for storing all words, and L is the depth of recursion stack (max word length).

