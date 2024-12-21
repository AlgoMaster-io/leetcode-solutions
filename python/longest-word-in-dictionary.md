[Leetcode Problem 720: Longest Word in Dictionary](https://leetcode.com/problems/longest-word-in-dictionary/)

## Approaches
1. [Brute Force with Set Verification](#approach-1)
2. [Trie with Depth-First Search (DFS)](#approach-2)

---

### Approach 1: Brute Force with Set Verification

#### Intuition:
The problem can be solved by iterating through the list of words, checking if each word can be constructed by adding letters to another word that is also in the list. The easiest data structure to check for existence is a set. We need to find the longest word that can be built one character at a time by other words in the dictionary.

#### Implementation:

```python
def longestWord(words):
    # Create a set to store the words for efficient lookup
    word_set = set(words)
    # Initialize a variable to keep track of the longest word found
    longest = ""
    
    # Iterate over each word in the list
    for word in words:
        # Check if all prefixes (subsequences) of word exist in the set
        if all(word[:k] in word_set for k in range(1, len(word))):
            # Check if the current word should replace the longest one found
            if len(word) > len(longest) or (len(word) == len(longest) and word < longest):
                longest = word
    
    return longest
```

#### Time Complexity:
- **O(N * M):** Where N is the number of words, and M is the average length of a word. For each word, verifying prefix existence takes O(M).
  
#### Space Complexity:
- **O(N * M):** To store the words in a set.

---

### Approach 2: Trie with Depth-First Search (DFS)

#### Intuition:
Using a Trie structure can be beneficial as it allows efficient prefix checking. Insert all words into a Trie and then conduct a DFS, finding the deepest valid word which meets the problem's constraints. This will inherently yield the longest word since the search is conducted from root to leaves.

#### Implementation:

```python
class TrieNode:
    def __init__(self):
        self.children = {}
        self.is_end_of_word = False

def insert(root, word):
    node = root
    for char in word:
        if char not in node.children:
            node.children[char] = TrieNode()
        node = node.children[char]
    node.is_end_of_word = True

def dfs(node, path):
    global longest
    if len(path) > len(longest):
        longest = path
    for char in sorted(node.children.keys()):
        if node.children[char].is_end_of_word:
            dfs(node.children[char], path + char)

def longestWord(words):
    global longest
    longest = ""
    # Initialize the root of the Trie
    root = TrieNode()
    
    # Insert all words into the Trie
    for word in words:
        insert(root, word)
    
    # Start DFS from the root
    dfs(root, "")
    
    return longest
```

#### Time Complexity:
- **O(N * M + L):** Where N is the number of words, M is the average length of the words, and L is the number of nodes traversed in the Trie during DFS.

#### Space Complexity:
- **O(N * M):** Space for the Trie structure to store all the words.

These approaches provide incrementally optimized solutions to effectively derive the longest word in the dictionary that can be built incrementally using other words.

