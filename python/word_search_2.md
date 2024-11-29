# 212. [Word Search II](https://leetcode.com/problems/word-search-ii/)

## Approach 1: Trie + Backtracking

### Solution
python
```python
# Time Complexity: O(n * m * 4^l), where n and m are the dimensions of the board, and l is the average word length
# Space Complexity: O(w * l), where w is the number of words and l is the average length of a word in the dictionary

from collections import defaultdict

class TrieNode:
    def __init__(self):
        self.children = defaultdict(TrieNode)
        self.word = None  # Store the word ending at this node

class Solution:
    def findWords(self, board, words):
        # Build the Trie
        root = TrieNode()
        for word in words:
            current = root
            for char in word:
                current = current.children[char]
            current.word = word  # Mark the end of a word

        result = []
        for i in range(len(board)):
            for j in range(len(board[0])):
                self.backtrack(board, i, j, root, result)

        return result

    def backtrack(self, board, row, col, node, result):
        letter = board[row][col]
        current = node.children.get(letter)

        # If no Trie node exists, return
        if not current:
            return

        # Check if the current node contains a word
        if current.word:
            result.append(current.word)
            current.word = None  # Avoid duplicate entries

        # Mark the current cell as visited
        board[row][col] = '#'

        # Explore the neighbors
        rowOffsets = [-1, 1, 0, 0]
        colOffsets = [0, 0, -1, 1]
        for i in range(4):
            newRow = row + rowOffsets[i]
            newCol = col + colOffsets[i]
            if 0 <= newRow < len(board) and 0 <= newCol < len(board[0]):
                self.backtrack(board, newRow, newCol, current, result)

        # Restore the cell value
        board[row][col] = letter

        # Optimization: Remove the leaf Trie node
        if not current.children:
            node.children.pop(letter)
```

