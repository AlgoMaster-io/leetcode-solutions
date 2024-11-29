# 211. [Design Add and Search Words Data Structure](https://leetcode.com/problems/design-add-and-search-words-data-structure/)

## Approach: Trie with Backtracking for Search

### Solution
```java
// Time Complexity:
//   - addWord(): O(m), where m is the length of the word
//   - search(): O(m * 26^k), where m is the length of the word and k is the number of wildcards
// Space Complexity: O(n * m), where n is the number of words and m is the average word length

class WordDictionary {

    private TrieNode root;

    public WordDictionary() {
        root = new TrieNode(); // Initialize the root node
    }

    // Adds a word into the data structure
    public void addWord(String word) {
        TrieNode node = root;
        for (char c : word.toCharArray()) {
            if (!node.containsKey(c)) {
                node.put(c, new TrieNode()); // Create a new node if not already present
            }
            node = node.get(c); // Move to the next node
        }
        node.setEnd(); // Mark the end of the word
    }

    // Returns true if the word is in the data structure, allowing '.' as a wildcard
    public boolean search(String word) {
        return searchInNode(word, 0, root);
    }

    // Helper function for backtracking search
    private boolean searchInNode(String word, int index, TrieNode node) {
        if (node == null) {
            return false; // Base case: node doesn't exist
        }
        if (index == word.length()) {
            return node.isEnd(); // Check if the word ends at this node
        }

        char c = word.charAt(index);
        if (c == '.') {
            // If the current character is '.', check all possible children
            for (char nextChar = 'a'; nextChar <= 'z'; nextChar++) {
                if (searchInNode(word, index + 1, node.get(nextChar))) {
                    return true;
                }
            }
        } else {
            // If the current character is not '.', proceed with the specific child
            return searchInNode(word, index + 1, node.get(c));
        }
        return false; // No match found
    }
}

// Trie Node Class
class TrieNode {
    private TrieNode[] links;
    private final int R = 26; // Number of possible children ('a' to 'z')
    private boolean isEnd;

    public TrieNode() {
        links = new TrieNode[R];
    }

    public boolean containsKey(char c) {
        return links[c - 'a'] != null;
    }

    public TrieNode get(char c) {
        return links[c - 'a'];
    }

    public void put(char c, TrieNode node) {
        links[c - 'a'] = node;
    }

    public void setEnd() {
        isEnd = true;
    }

    public boolean isEnd() {
        return isEnd;
    }
}
```