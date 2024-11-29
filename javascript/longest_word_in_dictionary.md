# 720. [Longest Word in Dictionary](https://leetcode.com/problems/longest-word-in-dictionary/)

## Approach 1: Sort and HashSet

### Solution
```javascript
// Time Complexity: O(n * k), where n is the number of words and k is the average length of a word
// Space Complexity: O(n * k)
const longestWord = function(words) {
    words.sort(); // Sort words lexicographically
    const built = new Set();
    let result = "";

    for (const word of words) {
        // Check if the prefix (word without the last character) is in the set
        if (word.length === 1 || built.has(word.substring(0, word.length - 1))) {
            built.add(word); // Add the word to the set
            // Update result if the word is longer or lexicographically smaller
            if (word.length > result.length || 
                (word.length === result.length && word < result)) {
                result = word;
            }
        }
    }

    return result;
};
```

## Approach 2: Trie-Based Solution

### Solution
```javascript
// Time Complexity: O(n * k), where n is the number of words and k is the average length of a word
// Space Complexity: O(n * k)

class TrieNode {
    constructor() {
        this.children = new Map();
        this.isEndOfWord = false;
        this.word = "";
    }
}

const longestWord = function(words) {
    const root = new TrieNode();

    // Build the Trie
    for (const word of words) {
        let current = root;
        for (const char of word) {
            if (!current.children.has(char)) {
                current.children.set(char, new TrieNode());
            }
            current = current.children.get(char);
        }
        current.isEndOfWord = true;
        current.word = word;
    }

    // Perform DFS to find the longest word
    return dfs(root, "");
};

function dfs(node, result) {
    if (!node.isEndOfWord && node !== null) {
        return result;
    }

    // Check for the longest word in the current branch
    if (node.word.length > result.length || 
        (node.word.length === result.length && node.word < result)) {
        result = node.word;
    }

    for (let charCode = 'a'.charCodeAt(0); charCode <= 'z'.charCodeAt(0); charCode++) {
        const char = String.fromCharCode(charCode);
        if (node.children.has(char)) {
            result = dfs(node.children.get(char), result);
        }
    }

    return result;
}
```

