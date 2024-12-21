# [Leetcode 720: Longest Word in Dictionary](https://leetcode.com/problems/longest-word-in-dictionary/)

## Solutions:
- [Solution 1: Brute Force](#solution-1-brute-force)
- [Solution 2: Using a Set for Fast Lookup](#solution-2-using-a-set-for-fast-lookup)
- [Solution 3: Trie Structure Approach](#solution-3-trie-structure-approach)

### Solution 1: Brute Force

#### Intuition
The simplest solution is to iterate through each word and for each word, check if all its prefixes exist in the array. We then keep track of the longest word (or lexicographically smallest in case of ties).

#### Steps:
1. Sort the words array. Sorting helps to ensure we are considering words in lexicographical order, which helps in case of ties.
2. For each word, check if all prefixes of the word exist in the words array.
3. If all prefixes exist, check the length and update the longest word accordingly.

#### Implementation

```go
func longestWord(words []string) string {
    sort.Strings(words) // Step 1: Sort words lexicographically.
    maxWord := ""
    for _, word := range words { // Step 2: Iterate over sorted words.
        valid := true
        for i := 1; i < len(word); i++ {
            if !contains(words, word[:i]) { // Step 2a: Check for all prefixes.
                valid = false
                break
            }
        }
        if valid && (len(word) > len(maxWord)) { // Step 3: Update maxWord accordingly.
            maxWord = word
        }
    }
    return maxWord
}

func contains(words []string, s string) bool {
    for _, w := range words {
        if w == s {
            return true
        }
    }
    return false
}
```

#### Time Complexity
- Sorting the words takes \(O(n \log n)\).
- Checking all prefixes for each word makes it \(O(n \times k^2)\) where \(k\) is the maximum length of any word.

#### Space Complexity
- \(O(1)\) aside from the input words storage.

---

### Solution 2: Using a Set for Fast Lookup

#### Intuition
One can optimize the prefix search by utilizing a hash set to store and check prefixes in \(O(1)\) time.

#### Steps:
1. Store all words in a set for constant time lookup.
2. Iterate through each word and check for existence of all its prefixes using the set.
3. Track the longest valid word.

#### Implementation

```go
func longestWord(words []string) string {
    wordSet := make(map[string]bool)
    for _, word := range words { // Step 1: Add words to the set.
        wordSet[word] = true
    }

    maxWord := ""
    for _, word := range words {
        valid := true
        for i := 1; i < len(word); i++ { // Step 2: Check for all prefixes.
            if !wordSet[word[:i]] {
                valid = false
                break
            }
        }
        
        if valid && (len(word) > len(maxWord) || (len(word) == len(maxWord) && word < maxWord)) { // Step 3: Update maxWord.
            maxWord = word
        }
    }
    return maxWord
}
```

#### Time Complexity
- \(O(n \times k)\) due to prefix checking and set operations.
  
#### Space Complexity
- \(O(n \times k)\), where we use a hash set additionally.

---

### Solution 3: Trie Structure Approach

#### Intuition
A Trie is well-suited for problems involving prefix searching, allowing efficient traversal and validation of word builds from prefixes.

#### Steps:
1. Build a Trie of the words.
2. Use BFS or DFS to ensure each word is buildable from its prefixes.
3. Keep track of the longest valid word during traversal.

#### Implementation

```go
type TrieNode struct {
    children map[rune]*TrieNode
    isEndOfWord bool
}

func longestWord(words []string) string {
    root := &TrieNode{children: make(map[rune]*TrieNode)}
    
    // Step 1: Build Trie
    for _, word := range words {
        current := root
        for _, char := range word {
            if _, exists := current.children[char]; !exists {
                current.children[char] = &TrieNode{children: make(map[rune]*TrieNode)}
            }
            current = current.children[char]
        }
        current.isEndOfWord = true
    }
    
    // Step 2: Use DFS to find the longest word
    var dfs func(*TrieNode, string) string
    dfs = func(node *TrieNode, word string) string {
        longest := word
        for char, child := range node.children {
            if child.isEndOfWord {
                candidate := dfs(child, word+string(char))
                if len(candidate) > len(longest) || (len(candidate) == len(longest) && candidate < longest) {
                    longest = candidate
                }
            }
        }
        return longest
    }
    
    return dfs(root, "")
}

```

#### Time Complexity
- Constructing the Trie takes \(O(n \times k)\) and searching it takes \(O(n \times k)\) in the worst case.

#### Space Complexity
- The Trie storage takes \(O(n \times k)\).

