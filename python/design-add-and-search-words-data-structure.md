# [Leetcode 211: Design Add and Search Words Data Structure](https://leetcode.com/problems/design-add-and-search-words-data-structure/)

## Table of Contents
- [Approach 1: Brute Force Using List](#approach-1-brute-force-using-list)
- [Approach 2: Trie-Based Approach](#approach-2-trie-based-approach)

## Approach 1: Brute Force Using List 

### Intuition
The simplest way to solve this problem is by storing all the added words in a list and then checking each word against the search query. For a search query containing a `.` character, we need to consider it as a wildcard character that matches any letter. 

### Solution

```python
class WordDictionary:

    def __init__(self):
        # Initialize the WordDictionary with an empty list to store words
        self.words = []

    def addWord(self, word: str) -> None:
        # Adds a word to the list
        self.words.append(word)

    def search(self, word: str) -> bool:
        # Check each word in the list
        for w in self.words:
            # Check if the word matches the search word, considering '.' as a wildcard
            if len(w) == len(word) and all(c1 == c2 or c2 == '.' for c1, c2 in zip(w, word)):
                return True
        return False
```

### Complexity Analysis
- **Time Complexity**: 
  - `addWord`: O(1) - simply appends the word to a list. 
  - `search`: O(n * m) - where `n` is the number of words and `m` is the average length of the words. Each search query scans all words in the list.
- **Space Complexity**: O(n * m) - where `n` is the number of words and `m` is the average word length due to storage of words in the list.

## Approach 2: Trie-Based Approach

### Intuition
A more efficient way to handle this problem is using a Trie (prefix tree). The Trie will allow us to store words in a structured way, making it efficient to search with wildcard match support.

### Solution

```python
class TrieNode:
    def __init__(self):
        # Initialize the TrieNode with a dictionary to store children nodes and a boolean indicating if it is the end of a word
        self.children = {}
        self.isEndOfWord = False

class WordDictionary:
    
    def __init__(self):
        # Initialize the WordDictionary with a root node of a Trie
        self.root = TrieNode()

    def addWord(self, word: str) -> None:
        # Add a word to the Trie
        current = self.root
        for char in word:
            if char not in current.children:
                # If character not in current node, add it to the node's children
                current.children[char] = TrieNode()
            current = current.children[char]
        # Mark the end of a word
        current.isEndOfWord = True

    def search(self, word: str) -> bool:
        # Search for a word in the Trie, supporting '.' as a wildcard character
        return self._search_in_node(word, self.root)

    def _search_in_node(self, word: str, node: TrieNode) -> bool:
        # Helper function to search from a given node
        for i, char in enumerate(word):
            if char == '.':
                # For '.', check all possible nodes at this character
                for child in node.children.values():
                    if self._search_in_node(word[i+1:], child):
                        return True
                return False
            else:
                # Check if character is in current node's children
                if char not in node.children:
                    return False
                # Move to the next node
                node = node.children[char]
        return node.isEndOfWord
```

### Complexity Analysis
- **Time Complexity**: 
  - `addWord`: O(m) - where `m` is the length of the word to be added.
  - `search`: Worst-case O(n * m) in balanced tree parameters, where `n` is the number of nodes and `m` is the length of the search word.
- **Space Complexity**: O(n * m) - where `n` is the number of words and `m` is the average word length, due to storage in the trie.

The trie-based approach significantly improves the search performance by leveraging the structured properties of the trie, especially when dealing with a large dataset of words.

