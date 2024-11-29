# 208. [Implement Trie (Prefix Tree)](https://leetcode.com/problems/implement-trie-prefix-tree/)

## Approach: Trie Node Implementation

### Solution
go
```go
// Time Complexity:
//   - insert(): O(m), where m is the length of the word
//   - search(): O(m), where m is the length of the word
//   - startsWith(): O(m), where m is the length of the prefix
// Space Complexity: O(n * m), where n is the number of words and m is the average word length

type Trie struct {
    Root *TrieNode
}

func Constructor() Trie {
    return Trie{Root: &TrieNode{Links: [26]*TrieNode{}}}
}

// Inserts a word into the trie
func (this *Trie) Insert(word string) {
    node := this.Root
    for _, c := range word {
        if !node.containsKey(byte(c)) {
            node.put(byte(c), &TrieNode{Links: [26]*TrieNode{}})
        }
        node = node.get(byte(c))
    }
    node.setEnd()
}

// Returns true if the word is in the trie
func (this *Trie) Search(word string) bool {
    node := this.searchPrefix(word)
    return node != nil && node.isEnd()
}

// Returns true if there is any word in the trie that starts with the given prefix
func (this *Trie) StartsWith(prefix string) bool {
    return this.searchPrefix(prefix) != nil
}

// Helper function to search for a node matching the prefix
func (this *Trie) searchPrefix(prefix string) *TrieNode {
    node := this.Root
    for _, c := range prefix {
        if node.containsKey(byte(c)) {
            node = node.get(byte(c))
        } else {
            return nil
        }
    }
    return node
}

// Trie Node Structure
type TrieNode struct {
    Links [26]*TrieNode
    IsEnd bool
}

func (this *TrieNode) containsKey(c byte) bool {
    return this.Links[c-'a'] != nil
}

func (this *TrieNode) get(c byte) *TrieNode {
    return this.Links[c-'a']
}

func (this *TrieNode) put(c byte, node *TrieNode) {
    this.Links[c-'a'] = node
}

func (this *TrieNode) setEnd() {
    this.IsEnd = true
}

func (this *TrieNode) isEnd() bool {
    return this.IsEnd
}
```

