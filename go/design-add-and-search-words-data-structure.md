[211. Design Add and Search Words Data Structure](https://leetcode.com/problems/design-add-and-search-words-data-structure/)

## Approaches
- [Trie-based Approach](#trie-based-approach)
- [Optimal Trie with DFS](#optimal-trie-with-dfs)

### Trie-based Approach

#### Intuition
The problem requires designing a data structure that supports adding new words and searching for them. The search function should be able to handle wildcard characters represented by '.'. A Trie (prefix tree) is an optimal choice for this type of operation because it allows quick insertion and search functionalities based on prefixes.

#### Approach
1. Construct a Trie where each node holds a map to its child nodes and a boolean indicating if it's the end of a word.
2. For the `addWord` function, iterate over the characters of the word and construct the Trie nodes as needed.
3. For the `search` function, traverse through the Trie node by node. If a '.', recursively check all possible nodes at that depth since '.' can represent any character.

```go
type TrieNode struct {
    children map[rune]*TrieNode
    isEndOfWord bool
}

type WordDictionary struct {
    root *TrieNode
}

func Constructor() WordDictionary {
    return WordDictionary{
        root: &TrieNode{
            children: make(map[rune]*TrieNode),
        },
    }
}

// Adds a word into the data structure.
func (this *WordDictionary) AddWord(word string)  {
    currentNode := this.root
    for _, ch := range word {
        // If current character is not present in the children, add it
        if _, exists := currentNode.children[ch]; !exists {
            currentNode.children[ch] = &TrieNode{
                children: make(map[rune]*TrieNode),
            }
        }
        // Move to the child node
        currentNode = currentNode.children[ch]
    }
    // Mark the end of the word
    currentNode.isEndOfWord = true
}

// Returns if the word is in the data structure. A word could contain the dot character '.' to represent any one letter.
func (this *WordDictionary) Search(word string) bool {
    return this.searchHelper(word, this.root)
}

// Helper function for searching
func (this *WordDictionary) searchHelper(word string, node *TrieNode) bool {
    currentNode := node
    for i, ch := range word {
        if ch == '.' {
            // '.' can be any character, so try each child
            for _, childNode := range currentNode.children {
                if this.searchHelper(word[i+1:], childNode) {
                    return true
                }
            }
            return false
        } else {
            if _, exists := currentNode.children[ch]; !exists {
                return false
            }
            // Move to the next node
            currentNode = currentNode.children[ch]
        }
    }
    return currentNode.isEndOfWord
}
```

#### Time Complexity
- `addWord`: O(N) where N is the length of the word.
- `search`: Worst case O(26^M) where M is the length of the word (each '.' could branch 26 ways).

#### Space Complexity
- O(N*M), where N is the number of words, and M is the average length of the words due to storing each character in the trie.

### Optimal Trie with DFS

#### Intuition
Improving performance by using Depth-First Search (DFS) and efficiently managing recursive calls using a helper function. Instead of exploring all paths for '.', early termination is possible as soon as a valid path is found.

#### Approach
The algorithm remains similar, but now the traversal during search explicitly handles the '.' wildcard by exploring each node only if required, improving both readability and speed.

The code implementation remains optimized similarly to above, ensuring early returns in recursive DFS to avoid unnecessary computations.

This approach keeps the logic compact and optimized for both space and runtime.

Time and space complexities remain the same as the Trie-based approach.

### Conclusion
Using a Trie is an efficient way to solve this problem by leveraging the data structure's property to handle prefix-based insertions and searches efficiently. The wildcard search through recursion provides flexibility to adhere to the problem's constraints.

