# 211. [Design Add and Search Words Data Structure](https://leetcode.com/problems/design-add-and-search-words-data-structure/)

## Approach: Trie with Backtracking for Search

### Solution
```python
# Time Complexity:
#   - addWord(): O(m), where m is the length of the word
#   - search(): O(m * 26^k), where m is the length of the word and k is the number of wildcards
# Space Complexity: O(n * m), where n is the number of words and m is the average word length

class TrieNode:
    def __init__(self):
        self.links = [None] * 26
        self.is_end = False

    def contains_key(self, ch: str) -> bool:
        return self.links[ord(ch) - ord('a')] is not None

    def get(self, ch: str) -> 'TrieNode':
        return self.links[ord(ch) - ord('a')]

    def put(self, ch: str, node: 'TrieNode') -> None:
        self.links[ord(ch) - ord('a')] = node

    def set_end(self) -> None:
        self.is_end = True

    def is_end(self) -> bool:
        return self.is_end


class WordDictionary:

    def __init__(self):
        self.root = TrieNode()  # Initialize the root node

    def addWord(self, word: str) -> None:
        node = self.root
        for c in word:
            if not node.contains_key(c):
                node.put(c, TrieNode())  # Create a new node if not already present
            node = node.get(c)  # Move to the next node
        node.set_end()  # Mark the end of the word

    def search(self, word: str) -> bool:
        return self._search_in_node(word, 0, self.root)

    def _search_in_node(self, word: str, index: int, node: TrieNode) -> bool:
        if node is None:
            return False  # Base case: node doesn't exist
        if index == len(word):
            return node.is_end()  # Check if the word ends at this node

        c = word[index]
        if c == '.':
            # If the current character is '.', check all possible children
            for next_char_index in range(26):
                if self._search_in_node(word, index + 1, node.links[next_char_index]):
                    return True
        else:
            # If the current character is not '.', proceed with the specific child
            return self._search_in_node(word, index + 1, node.get(c))
        return False  # No match found
```


