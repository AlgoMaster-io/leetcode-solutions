# 212. [Word Search II](https://leetcode.com/problems/word-search-ii/)

## Approach 1: Trie + Backtracking

### Solution
Go
```go
// Time Complexity: O(n * m * 4^l), where n and m are the dimensions of the board, and l is the average word length
// Space Complexity: O(w * l), where w is the number of words and l is the average length of a word in the dictionary
package main

type TrieNode struct {
    children map[rune]*TrieNode
    word     string // Store the word ending at this node
}

func findWords(board [][]byte, words []string) []string {
    // Build the Trie
    root := &TrieNode{children: map[rune]*TrieNode{}}
    for _, word := range words {
        current := root
        for _, c := range word {
            if _, found := current.children[c]; !found {
                current.children[c] = &TrieNode{children: map[rune]*TrieNode{}}
            }
            current = current.children[c]
        }
        current.word = word // Mark the end of a word
    }

    result := []string{}
    for i := 0; i < len(board); i++ {
        for j := 0; j < len(board[0]); j++ {
            backtrack(board, i, j, root, &result)
        }
    }

    return result
}

func backtrack(board [][]byte, row int, col int, node *TrieNode, result *[]string) {
    letter := rune(board[row][col])
    current, exists := node.children[letter]
    // If no Trie node exists, return
    if !exists {
        return
    }

    // Check if the current node contains a word
    if current.word != "" {
        *result = append(*result, current.word)
        current.word = "" // Avoid duplicate entries
    }

    // Mark the current cell as visited
    board[row][col] = '#'

    // Explore the neighbors
    rowOffsets := []int{-1, 1, 0, 0}
    colOffsets := []int{0, 0, -1, 1}
    for i := 0; i < 4; i++ {
        newRow := row + rowOffsets[i]
        newCol := col + colOffsets[i]
        if newRow >= 0 && newRow < len(board) && newCol >= 0 && newCol < len(board[0]) {
            backtrack(board, newRow, newCol, current, result)
        }
    }

    // Restore the cell value
    board[row][col] = byte(letter)

    // Optimization: Remove the leaf Trie node
    if len(current.children) == 0 {
        delete(node.children, letter)
    }
}
```

