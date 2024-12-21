# [Leetcode 211: Design Add and Search Words Data Structure](https://leetcode.com/problems/design-add-and-search-words-data-structure/)

## Approaches:
- [Solution 1: Brute Force Approach](#solution-1-brute-force-approach)
- [Solution 2: Trie Implementation](#solution-2-trie-implementation)

### Solution 1: Brute Force Approach

#### Intuition
We can store each added word in a list and when a search operation is called, we'll iterate through all the stored words to check if there's any match to the search word. For search words having a wildcard character '.', we need to check each character except the wildcard positions. This approach is inefficient for large datasets due to the linear search which is coupled with string matching complexity.

#### Code
```java
class WordDictionary {
    private List<String> words;

    public WordDictionary() {
        words = new ArrayList<>(); // Initialize the list to store words
    }

    public void addWord(String word) {
        words.add(word); // Add the word to the list
    }

    public boolean search(String word) {
        for (String s : words) { // Iterate over each added word
            if (match(s, word)) { // Check if the word matches the search word
                return true;
            }
        }
        return false; // No matching word found
    }

    private boolean match(String s, String word) {
        if (s.length() != word.length()) return false; // Length must be same

        for (int i = 0; i < s.length(); i++) {
            if (word.charAt(i) != '.' && word.charAt(i) != s.charAt(i)) {
                return false; // Mismatch found unless character is '.'
            }
        }
        return true; // All characters matched
    }
}
```

#### Complexity Analysis
- **Time Complexity**: O(n * m) for search where n is the number of words and m is the average length of words.
- **Space Complexity**: O(n) to store the list of words.

### Solution 2: Trie Implementation

#### Intuition
A more efficient approach is to use a Trie (prefix tree) to store the added words. The trie allows us to efficiently search by traversing down the tree character by character, matching the structure of the search word. For each wildcard '.', we can traverse down all possible paths. This significantly reduces the time complexity for search operations compared to the brute force method.

#### Code
```java
class WordDictionary {
    private static class TrieNode {
        private TrieNode[] children;
        private boolean isEndOfWord;

        public TrieNode() {
            children = new TrieNode[26]; // 26 children for each letter 'a' to 'z'
            isEndOfWord = false; // True if the node represents the end of a word
        }
    }

    private TrieNode root;

    public WordDictionary() {
        root = new TrieNode(); // Initialize root
    }

    public void addWord(String word) {
        TrieNode node = root;
        for (char c : word.toCharArray()) {
            int index = c - 'a';
            if (node.children[index] == null) {
                node.children[index] = new TrieNode(); // Create a node if not exist
            }
            node = node.children[index];
        }
        node.isEndOfWord = true; // Mark the last node as end of a word
    }

    public boolean search(String word) {
        return searchInNode(word, root, 0);
    }

    private boolean searchInNode(String word, TrieNode node, int pos) {
        if (pos == word.length()) {
            return node.isEndOfWord; // Return true if end of word is reached
        }

        char c = word.charAt(pos);
        if (c == '.') {
            for (TrieNode child : node.children) { 
                if (child != null && searchInNode(word, child, pos + 1)) {
                    return true; // Match with any possible letter
                }
            }
            return false; // No valid match found
        } else {
            int index = c - 'a';
            return node.children[index] != null && searchInNode(word, node.children[index], pos + 1);
        }
    }
}
```

#### Complexity Analysis
- **Time Complexity**: O(m) for adding and typical search where m is the length of the word.
- **Space Complexity**: O(n * m) where n is the number of words and m is the average length of words to store the Trie.

