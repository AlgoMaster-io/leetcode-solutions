# 720. [Longest Word in Dictionary](https://leetcode.com/problems/longest-word-in-dictionary/)

## Approach 1: Sort and HashSet

### Solution
```python
# Time Complexity: O(n * k), where n is the number of words and k is the average length of a word
# Space Complexity: O(n * k)
class Solution:
    def longestWord(self, words):
        words.sort()  # Sort words lexicographically
        built = set()
        result = ""

        for word in words:
            # Check if the prefix (word without the last character) is in the set
            if len(word) == 1 or word[:-1] in built:
                built.add(word)  # Add the word to the set
                # Update result if the word is longer or lexicographically smaller
                if len(word) > len(result) or (len(word) == len(result) and word < result):
                    result = word

        return result
```

## Approach 2: Trie-Based Solution

### Solution
```python
# Time Complexity: O(n * k), where n is the number of words and k is the average length of a word
# Space Complexity: O(n * k)
class TrieNode:
    def __init__(self):
        self.children = {}
        self.isEndOfWord = False
        self.word = ""

class Solution:
    def longestWord(self, words):
        root = TrieNode()

        # Build the Trie
        for word in words:
            current = root
            for c in word:
                if c not in current.children:
                    current.children[c] = TrieNode()
                current = current.children[c]
            current.isEndOfWord = True
            current.word = word

        # Perform DFS to find the longest word
        return self.dfs(root, "")

    def dfs(self, node, result):
        if node is None or (not node.isEndOfWord and node is not None):
            return result

        # Check for the longest word in the current branch
        if len(node.word) > len(result) or (len(node.word) == len(result) and node.word < result):
            result = node.word

        for c in range(ord('a'), ord('z') + 1):
            result = self.dfs(node.children.get(chr(c)), result)

        return result
```

