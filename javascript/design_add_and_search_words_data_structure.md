# 211. [Design Add and Search Words Data Structure](https://leetcode.com/problems/design-add-and-search-words-data-structure/)

## Approach: Trie with Backtracking for Search

### Solution
javascript
```javascript
// Time Complexity:
//   - addWord(): O(m), where m is the length of the word
//   - search(): O(m * 26^k), where m is the length of the word and k is the number of wildcards
// Space Complexity: O(n * m), where n is the number of words and m is the average word length

class TrieNode {
    constructor() {
        this.links = Array(26).fill(null);
        this.isEnd = false;
    }

    containsKey(ch) {
        return this.links[ch.charCodeAt(0) - 'a'.charCodeAt(0)] !== null;
    }

    get(ch) {
        return this.links[ch.charCodeAt(0) - 'a'.charCodeAt(0)];
    }

    put(ch, node) {
        this.links[ch.charCodeAt(0) - 'a'.charCodeAt(0)] = node;
    }

    setEnd() {
        this.isEnd = true;
    }

    isEnd() {
        return this.isEnd;
    }
}

class WordDictionary {
    constructor() {
        this.root = new TrieNode(); // Initialize the root node
    }

    // Adds a word into the data structure
    addWord(word) {
        let node = this.root;
        for (let i = 0; i < word.length; i++) {
            let c = word[i];
            if (!node.containsKey(c)) {
                node.put(c, new TrieNode()); // Create a new node if not already present
            }
            node = node.get(c); // Move to the next node
        }
        node.setEnd(); // Mark the end of the word
    }

    // Returns true if the word is in the data structure, allowing '.' as a wildcard
    search(word) {
        return this.searchInNode(word, 0, this.root);
    }

    // Helper function for backtracking search
    searchInNode(word, index, node) {
        if (node === null) {
            return false; // Base case: node doesn't exist
        }
        if (index === word.length) {
            return node.isEnd(); // Check if the word ends at this node
        }

        let c = word[index];
        if (c === '.') {
            // If the current character is '.', check all possible children
            for (let nextCharCode = 'a'.charCodeAt(0); nextCharCode <= 'z'.charCodeAt(0); nextCharCode++) {
                if (this.searchInNode(word, index + 1, node.get(String.fromCharCode(nextCharCode)))) {
                    return true;
                }
            }
        } else {
            // If the current character is not '.', proceed with the specific child
            return this.searchInNode(word, index + 1, node.get(c));
        }
        return false; // No match found
    }
}
```

