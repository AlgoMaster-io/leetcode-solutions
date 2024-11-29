# 208. [Implement Trie (Prefix Tree)](https://leetcode.com/problems/implement-trie-prefix-tree/)

## Approach: Trie Node Implementation

### Solution
```java
// Time Complexity:
//   - insert(): O(m), where m is the length of the word
//   - search(): O(m), where m is the length of the word
//   - startsWith(): O(m), where m is the length of the prefix
// Space Complexity: O(n * m), where n is the number of words and m is the average word length

class Trie {

    private TrieNode root;

    public Trie() {
        root = new TrieNode(); // Initialize the root node
    }

    // Inserts a word into the trie
    public void insert(String word) {
        TrieNode node = root;
        for (char c : word.toCharArray()) {
            if (!node.containsKey(c)) {
                node.put(c, new TrieNode()); // Create a new node if not already present
            }
            node = node.get(c); // Move to the next node
        }
        node.setEnd(); // Mark the end of the word
    }

    // Returns true if the word is in the trie
    public boolean search(String word) {
        TrieNode node = searchPrefix(word);
        return node != null && node.isEnd(); // Check if the word ends at this node
    }

    // Returns true if there is any word in the trie that starts with the given prefix
    public boolean startsWith(String prefix) {
        return searchPrefix(prefix) != null;
    }

    // Helper function to search for a node matching the prefix
    private TrieNode searchPrefix(String prefix) {
        TrieNode node = root;
        for (char c : prefix.toCharArray()) {
            if (node.containsKey(c)) {
                node = node.get(c); // Move to the next node
            } else {
                return null; // Prefix not found
            }
        }
        return node; // Return the last node of the prefix
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