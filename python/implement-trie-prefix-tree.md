# [Leetcode 208: Implement Trie (Prefix Tree)](https://leetcode.com/problems/implement-trie-prefix-tree/)

## Approaches:
1. [Basic Approach: Using a Set to Store Words](#approach-1-basic-approach-using-a-set-to-store-words)
2. [Optimal Approach: Using Trie with Nested Dictionaries](#approach-2-optimal-approach-using-trie-with-nested-dictionaries)

### Approach 1: Basic Approach: Using a Set to Store Words

**Intuition:**

The basic method to support operations like insert, search, and startsWith is to utilize a data structure that inherently supports these operations efficiently, such as a `set`. This approach is simple and quick, leveraging Python's built-in capabilities to store data. However, it won't optimize for space or provide the real benefits of a trie structure, which is efficient representation for large datasets with shared prefixes.

**Steps:**

- Use a `set` to insert words, check if a word exists, and check if any word in the set starts with a given prefix.
- For `startsWith`, iterate through each word to check if it starts with the prefix (this is not efficient and doesn't leverage trie properties).

**Code:**

```python
class Trie:
    def __init__(self):
        # Initialize an empty set to store dictionary words
        self.words_set = set()

    def insert(self, word: str) -> None:
        # Add the word directly to the set
        self.words_set.add(word)

    def search(self, word: str) -> bool:
        # Check if the word exists in the set
        return word in self.words_set

    def startsWith(self, prefix: str) -> bool:
        # Check if any word starts with the given prefix
        for word in self.words_set:
            if word.startswith(prefix):
                return True
        return False
```

**Complexity:**

- **Time Complexity:** Insert and search operations are average O(1) for HashSet-based operations.
- **Space Complexity:** O(N * M), where N is the number of words, and M is the maximum word length.

### Approach 2: Optimal Approach: Using Trie with Nested Dictionaries

**Intuition:**

A trie (pronounced as "try") is a tree-like data structure that efficiently stores a dynamic set of strings, especially when there is a common prefix among the strings. Here, each path down the tree represents a string, and nodes along the way denote ways to build strings with shared prefixes.

**Steps:**

- Use a dictionary to represent the trie node. Each node contains a dictionary to point to child nodes and a boolean to indicate if a node is the end of a word.
- For `insert`, traverse the trie for each character. If a character is not present, add it.
- For `search`, traverse the trie, checking at each level if the node exists. Only return `true` if all characters are visited and the last node completes a word.
- For `startsWith`, traverse the trie checking if a path exists for the prefix.

**Code:**

```python
class TrieNode:
    def __init__(self):
        # Child nodes and a flag to mark the end of a word
        self.children = {}
        self.is_end_of_word = False

class Trie:
    def __init__(self):
        # The root of our Trie is an empty TrieNode
        self.root = TrieNode()

    def insert(self, word: str) -> None:
        current = self.root
        for char in word:
            # If the character is not in the children, add a new TrieNode
            if char not in current.children:
                current.children[char] = TrieNode()
            # Move to the next node in the tree
            current = current.children[char]
        # Mark the end of a complete word
        current.is_end_of_word = True
        
    def search(self, word: str) -> bool:
        current = self.root
        for char in word:
            # If the character is not found, the word does not exist
            if char not in current.children:
                return False
            current = current.children[char]
        # Check if it's the end of a word
        return current.is_end_of_word

    def startsWith(self, prefix: str) -> bool:
        current = self.root
        for char in prefix:
            # If a character in prefix is not found, no word with such prefix
            if char not in current.children:
                return False
            current = current.children[char]
        # All characters in prefix found, hence a valid prefix
        return True
```

**Complexity:**

- **Time Complexity:** O(M) for each operation, where M is the length of the word or prefix being operated upon.
- **Space Complexity:** O(N * M), where N is the number of words inserted, and M is the average length of the words (since we store nodes for each character). 

This optimal approach truly represents the characteristics of a trie, providing efficient operations for both insertion and lookups while minimizing space usage by exploiting common prefixes.

