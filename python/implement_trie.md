# 208. [Implement Trie (Prefix Tree)](https://leetcode.com/problems/implement-trie-prefix-tree/)

## Approach: Trie Node Implementation

### Solution
```python
# Time Complexity:
#   - insert(): O(m), where m is the length of the word
#   - search(): O(m), where m is the length of the word
#   - startsWith(): O(m), where m is the length of the prefix
# Space Complexity: O(n * m), where n is the number of words and m is the average word length

class TrieNode:
    def __init__(self):
        self.R = 26  # Number of possible children ('a' to 'z')
        self.links = [None] * self.R
        self.is_end = False

    def contains_key(self, char):
        return self.links[ord(char) - ord('a')] is not None

    def get(self, char):
        return self.links[ord(char) - ord('a')]

    def put(self, char, node):
        self.links[ord(char) - ord('a')] = node

    def set_end(self):
        self.is_end = True

    def is_end(self):
        return self.is_end


class Trie:
    def __init__(self):
        self.root = TrieNode()  # Initialize the root node

    def insert(self, word):
        node = self.root
        for char in word:
            if not node.contains_key(char):
                node.put(char, TrieNode())  # Create a new node if not already present
            node = node.get(char)  # Move to the next node
        node.set_end()  # Mark the end of the word

    def search(self, word):
        node = self._search_prefix(word)
        return node is not None and node.is_end  # Check if the word ends at this node

    def starts_with(self, prefix):
        return self._search_prefix(prefix) is not None

    def _search_prefix(self, prefix):
        node = self.root
        for char in prefix:
            if node.contains_key(char):
                node = node.get(char)  # Move to the next node
            else:
                return None  # Prefix not found
        return node  # Return the last node of the prefix
```

