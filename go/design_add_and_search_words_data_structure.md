# 211. [Design Add and Search Words Data Structure](https://leetcode.com/problems/design-add-and-search-words-data-structure/)

## Approach: Trie with Backtracking for Search

### Solution
```go
// Time Complexity:
//   - addWord(): O(m), where m is the length of the word
//   - search(): O(m * 26^k), where m is the length of the word and k is the number of wildcards
// Space Complexity: O(n * m), where n is the number of words and m is the average word length

package main

type TrieNode struct {
    links [26]*TrieNode // Array to hold children nodes
    isEnd bool          // To check if it's the end of a word
}

func (node *TrieNode) containsKey(c byte) bool {
    return node.links[c-'a'] != nil
}

func (node *TrieNode) get(c byte) *TrieNode {
    return node.links[c-'a']
}

func (node *TrieNode) put(c byte, child *TrieNode) {
    node.links[c-'a'] = child
}

func (node *TrieNode) setEnd() {
    node.isEnd = true
}

func (node *TrieNode) isEndNode() bool {
    return node.isEnd
}

type WordDictionary struct {
    root *TrieNode
}

func Constructor() WordDictionary {
    return WordDictionary{root: &TrieNode{}} // Initialize the root node
}

func (this *WordDictionary) AddWord(word string) {
    node := this.root
    for i := 0; i < len(word); i++ {
        c := word[i]
        if !node.containsKey(c) {
            node.put(c, &TrieNode{}) // Create a new node if not already present
        }
        node = node.get(c) // Move to the next node
    }
    node.setEnd() // Mark the end of the word
}

func (this *WordDictionary) Search(word string) bool {
    return this.searchInNode(word, 0, this.root)
}

func (this *WordDictionary) searchInNode(word string, index int, node *TrieNode) bool {
    if node == nil {
        return false // Base case: node doesn't exist
    }
    if index == len(word) {
        return node.isEndNode() // Check if the word ends at this node
    }

    c := word[index]
    if c == '.' {
        // If the current character is '.', check all possible children
        for nextChar := 0; nextChar < 26; nextChar++ {
            if this.searchInNode(word, index+1, node.links[nextChar]) {
                return true
            }
        }
    } else {
        // If the current character is not '.', proceed with the specific child
        return this.searchInNode(word, index+1, node.get(c))
    }
    return false // No match found
}
```


