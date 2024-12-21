# [Leetcode 208: Implement Trie (Prefix Tree)](https://leetcode.com/problems/implement-trie-prefix-tree/)

## Table of Contents
- [Approach 1: Naive Tree Data Structure](#approach-1-naive-tree-data-structure)
- [Approach 2: Optimized Trie Implementation](#approach-2-optimized-trie-implementation)

### Approach 1: Naive Tree Data Structure

#### Intuition
A trie, or prefix tree, is a tree data structure used to efficiently store a set of strings where keys are usually strings. A typical way to store a string in a trie is to store each character as a separate node. The trie begins with a single root node, and each character of a string represents an edge that leads from one node to another.

#### Implementation
In this naive approach, we use a simple data structure where each node consists of a map where each character points to the corresponding child node.

```go
type TrieNode struct {
    children map[rune]*TrieNode
    isEndOfWord bool
}

type Trie struct {
    root *TrieNode
}

func Constructor() Trie {
    return Trie{root: &TrieNode{children: make(map[rune]*TrieNode)}}
}

func (t *Trie) Insert(word string) {
    currentNode := t.root
    for _, char := range word {
        // Check if the current character is in the map; if not, add it.
        if _, exists := currentNode.children[char]; !exists {
            currentNode.children[char] = &TrieNode{children: make(map[rune]*TrieNode)}
        }
        currentNode = currentNode.children[char]
    }
    // Mark the end of a word.
    currentNode.isEndOfWord = true
}

func (t *Trie) Search(word string) bool {
    currentNode := t.root
    for _, char := range word {
        if _, exists := currentNode.children[char]; !exists {
            return false
        }
        currentNode = currentNode.children[char]
    }
    return currentNode.isEndOfWord
}

func (t *Trie) StartsWith(prefix string) bool {
    currentNode := t.root
    for _, char := range prefix {
        if _, exists := currentNode.children[char]; !exists {
            return false
        }
        currentNode = currentNode.children[char]
    }
    return true
}
```

#### Complexity Analysis
- Time Complexity: O(m) per operation for insert, search, and starts with (where m is the key length).
- Space Complexity: O(n * m), where n is the number of total keys and m is the average length of the keys.

### Approach 2: Optimized Trie Implementation

#### Intuition
To optimize space, we replace the map data structure with a fixed size array representing each alphabet's corresponding position. This exploits the constant alphabet size (typically 26 for English lowercase letters). This approach uses less space than the naive map-based approach since the array size is constant, although this sacrifices flexibility as it assumes a fixed character set.

#### Implementation

```go
type OptimizedTrieNode struct {
    children [26]*OptimizedTrieNode
    isEndOfWord bool
}

type OptimizedTrie struct {
    root *OptimizedTrieNode
}

func OptimizedConstructor() OptimizedTrie {
    return OptimizedTrie{root: &OptimizedTrieNode{}}
}

func (t *OptimizedTrie) Insert(word string) {
    currentNode := t.root
    for _, char := range word {
        index := char - 'a'  // Calculate index for the character.
        if currentNode.children[index] == nil {
            currentNode.children[index] = &OptimizedTrieNode{}
        }
        currentNode = currentNode.children[index]
    }
    currentNode.isEndOfWord = true
}

func (t *OptimizedTrie) Search(word string) bool {
    currentNode := t.root
    for _, char := range word {
        index := char - 'a'
        if currentNode.children[index] == nil {
            return false
        }
        currentNode = currentNode.children[index]
    }
    return currentNode.isEndOfWord
}

func (t *OptimizedTrie) StartsWith(prefix string) bool {
    currentNode := t.root
    for _, char := range prefix {
        index := char - 'a'
        if currentNode.children[index] == nil {
            return false
        }
        currentNode = currentNode.children[index]
    }
    return true
}
```

#### Complexity Analysis
- Time Complexity: O(m) per operation for insert, search, and starts with, where m is the key length.
- Space Complexity: O(n * m), where n is the number of keys and m is the average key length. The space is more controlled due to the use of fixed-size arrays, potentially reducing overhead compared to maps for larger alphabets.

