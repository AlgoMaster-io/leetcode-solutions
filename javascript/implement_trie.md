# 208. [Implement Trie (Prefix Tree)](https://leetcode.com/problems/implement-trie-prefix-tree/)

## Approach: Trie Node Implementation

### Solution
```javascript
// Time Complexity:
//   - insert(): O(m), where m is the length of the word
//   - search(): O(m), where m is the length of the word
//   - startsWith(): O(m), where m is the length of the prefix
// Space Complexity: O(n * m), where n is the number of words and m is the average word length

class TrieNode {
    constructor() {
        this.links = new Array(26); // Array to hold children nodes for each letter
        this.isEnd = false; // Boolean to mark the end of a word
    }
    
    containsKey(char) {
        return this.links[char.charCodeAt(0) - 'a'.charCodeAt(0)] !== undefined;
    }
    
    get(char) {
        return this.links[char.charCodeAt(0) - 'a'.charCodeAt(0)];
    }
    
    put(char, node) {
        this.links[char.charCodeAt(0) - 'a'.charCodeAt(0)] = node;
    }
    
    setEnd() {
        this.isEnd = true;
    }
    
    isEnd() {
        return this.isEnd;
    }
}

class Trie {
    constructor() {
        this.root = new TrieNode(); // Initialize the root node
    }
    
    // Inserts a word into the trie
    insert(word) {
        let node = this.root;
        for (let char of word) {
            if (!node.containsKey(char)) {
                node.put(char, new TrieNode()); // Create a new node if not already present
            }
            node = node.get(char); // Move to the next node
        }
        node.setEnd(); // Mark the end of the word
    }
    
    // Returns true if the word is in the trie
    search(word) {
        const node = this.searchPrefix(word);
        return node !== null && node.isEnd(); // Check if the word ends at this node
    }
    
    // Returns true if there is any word in the trie that starts with the given prefix
    startsWith(prefix) {
        return this.searchPrefix(prefix) !== null;
    }
    
    // Helper function to search for a node matching the prefix
    searchPrefix(prefix) {
        let node = this.root;
        for (let char of prefix) {
            if (node.containsKey(char)) {
                node = node.get(char); // Move to the next node
            } else {
                return null; // Prefix not found
            }
        }
        return node; // Return the last node of the prefix
    }
}
```

