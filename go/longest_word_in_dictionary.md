# 720. [Longest Word in Dictionary](https://leetcode.com/problems/longest-word-in-dictionary/)

## Approach 1: Sort and HashSet

### Solution
go
```go
// Time Complexity: O(n * k), where n is the number of words and k is the average length of a word
// Space Complexity: O(n * k)
import (
	"sort"
)

func longestWord(words []string) string {
	// Sort words lexicographically
	sort.Strings(words)
	built := make(map[string]bool)
	result := ""

	for _, word := range words {
		// Check if the prefix (word without the last character) is in the set
		if len(word) == 1 || built[word[:len(word)-1]] {
			built[word] = true // Add the word to the set
			// Update result if the word is longer or lexicographically smaller
			if len(word) > len(result) || (len(word) == len(result) && word < result) {
				result = word
			}
		}
	}

	return result
}
```

## Approach 2: Trie-Based Solution

### Solution
go
```go
// Time Complexity: O(n * k), where n is the number of words and k is the average length of a word
// Space Complexity: O(n * k)
type TrieNode struct {
	children    map[byte]*TrieNode
	isEndOfWord bool
	word        string
}

func newTrieNode() *TrieNode {
	return &TrieNode{
		children: make(map[byte]*TrieNode),
	}
}

func longestWord(words []string) string {
	root := newTrieNode()

	// Build the Trie
	for _, word := range words {
		current := root
		for i := range word {
			char := word[i]
			if _, exists := current.children[char]; !exists {
				current.children[char] = newTrieNode()
			}
			current = current.children[char]
		}
		current.isEndOfWord = true
		current.word = word
	}

	// Perform DFS to find the longest word
	return dfs(root, "")
}

func dfs(node *TrieNode, result string) string {
	if node == nil || (!node.isEndOfWord && node != nil) {
		return result
	}

	// Check for the longest word in the current branch
	if len(node.word) > len(result) || (len(node.word) == len(result) && node.word < result) {
		result = node.word
	}

	for char := byte('a'); char <= 'z'; char++ {
		result = dfs(node.children[char], result)
	}

	return result
}
```

