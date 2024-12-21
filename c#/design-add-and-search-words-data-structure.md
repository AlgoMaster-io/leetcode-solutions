# [Leetcode 211: Design Add and Search Words Data Structure](https://leetcode.com/problems/design-add-and-search-words-data-structure/)

## Table of Contents
- [Approach 1: Brute Force using List](#approach-1-brute-force-using-list)
- [Approach 2: Using Trie](#approach-2-using-trie)

## Approach 1: Brute Force using List

### Intuition
The easiest way to handle the functionality of adding and searching words in the word dictionary is by using a list. Each added word is appended to the list, and for the search operation, iterate through each word to see if it matches the search condition. We can directly compare characters or substitute the `.` with any potential character for matching.

### Code
```csharp
public class WordDictionary {
    private List<string> words;

    public WordDictionary() {
        words = new List<string>();
    }
    
    public void AddWord(string word) {
        // Add the word to the list
        words.Add(word);
    }
    
    public bool Search(string word) {
        foreach(var w in words) {
            if (IsMatch(w, word)) {
                return true;
            }
        }
        return false;
    }
    
    private bool IsMatch(string word, string pattern) {
        if (word.Length != pattern.Length) return false;

        for (int i = 0; i < word.Length; i++) {
            // If characters do not match and pattern character is not '.'
            if (pattern[i] != '.' && word[i] != pattern[i]) {
                return false;
            }
        }
        return true;
    }
}
```

### Complexity Analysis
- **Time Complexity**: 
  - `AddWord`: O(1) for adding a word to the list.
  - `Search`: O(n * m) where n is the number of words, and m is the average length of the words.
- **Space Complexity**: O(n * m), storage required for storing the words.

## Approach 2: Using Trie

### Intuition
The trie (prefix tree) is a specialized data structure that is ideal for storing strings and has efficient search operations, especially useful when searching with wildcards like `.`. Each node in the trie represents a single character, and paths from the root to nodes represent words.

### Code
```csharp
public class WordDictionary {
    private class TrieNode {
        public Dictionary<char, TrieNode> Children = new Dictionary<char, TrieNode>();
        public bool IsEndOfWord = false;
    }
    
    private TrieNode root;
    
    public WordDictionary() {
        root = new TrieNode();
    }
    
    public void AddWord(string word) {
        TrieNode current = root;
        foreach (char ch in word) {
            if (!current.Children.ContainsKey(ch)) {
                current.Children[ch] = new TrieNode();
            }
            current = current.Children[ch];
        }
        current.IsEndOfWord = true;
    }
    
    public bool Search(string word) {
        return SearchInNode(word, root, 0);
    }
    
    private bool SearchInNode(string word, TrieNode node, int index) {
        if (index == word.Length) {
            return node.IsEndOfWord;
        }
        
        char ch = word[index];
        if (ch == '.') {
            foreach (var child in node.Children.Values) {
                if (SearchInNode(word, child, index + 1)) {
                    return true;
                }
            }
            return false;
        } else {
            if (!node.Children.ContainsKey(ch)) {
                return false;
            }
            return SearchInNode(word, node.Children[ch], index + 1);
        }
    }
}
```

### Complexity Analysis
- **Time Complexity**: 
  - `AddWord`: O(m) where m is the length of the word.
  - `Search`: O(m) in the average case; in the worst case (with `.`), it's O(26^m).
- **Space Complexity**: O(m * n) where m is the average length of words and n is the number of words, since each node allocates for each character.

