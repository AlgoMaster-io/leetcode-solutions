# [Leetcode Problem 208: Implement Trie (Prefix Tree)](https://leetcode.com/problems/implement-trie-prefix-tree/)

In this problem, we need to design a data structure called a "Trie" (pronounced as "try"). A Trie is a tree-like data structure used to efficiently store and search a dynamic set or associative array where the keys are usually strings. Your task is to implement three methods:

1. `insert(word)`: Inserts the string `word` into the trie.
2. `search(word)`: Returns `true` if the string `word` is in the trie and `false` otherwise.
3. `startsWith(prefix)`: Returns `true` if there is any string in the trie that starts with the given `prefix` and `false` otherwise.

## Navigation Links:
- [Naive Solution](#naive-solution)
- [Optimized Solution](#optimized-solution)

## Naive Solution

### Intuition:
The basic idea of a Trie is very straightforward. Each node of a Trie represents a single character of a string. By traversing out from the root to the leaves, we can spell out the strings that are stored in the Trie. 

For this naive implementation, each node will store its children in an array (fixed size 26 for lowercase letters) and an indicator to show when a string ends.

### Implementation:
```javascript
class Trie {
    constructor() {
        // A node represents each alphabet character
        this.root = {};
        // Root node has an end marking to determine if it's an end of a word
        this.isEndOfWord = false;
    }

    insert(word) {
        // Start from the root
        let node = this.root;
        for (let char of word) {
            // If the character is not a child of current node, create a new node
            if (!node[char]) node[char] = {};
            // Move to the child node
            node = node[char];
        }
        // Mark the end of the word
        node.isEndOfWord = true;
    }

    search(word) {
        let node = this.root;
        for (let char of word) {
            // Check if character is in the current node
            if (!node[char]) return false;
            // Move to the next child node
            node = node[char];
        }
        // Return true if we are at the end of the word
        return node.isEndOfWord === true;
    }

    startsWith(prefix) {
        let node = this.root;
        for (let char of prefix) {
            // Check each character in the prefix
            if (!node[char]) return false;
            node = node[char];
        }
        // If we traverse the full prefix without returning false, prefix exists
        return true;
    }
}

```
### Complexity:
- **Time Complexity:**
  - `insert`: O(n), where n is the length of the word.
  - `search`: O(n), where n is the length of the word.
  - `startsWith`: O(m), where m is the length of the prefix.
- **Space Complexity:** O(N*M), where N is the number of words inserted, and M is the average length of each word.

## Optimized Solution

### Intuition:
The basic implementation works fine. However, we can optimize it by using JavaScript's `Map` instead of plain objects. `Map` in JavaScript can be more efficient for keeping track of dynamic data as it directly supports various operations that can speed up our program.

### Implementation:
```javascript
class Trie {
    constructor() {
        this.root = new Map();
        this.isEndOfWord = false;
    }

    insert(word) {
        let node = this.root;
        for (let char of word) {
            if (!node.has(char)) {
                // Initialize a new map if character is not present
                node.set(char, new Map());
            }
            node = node.get(char);
        }
        // Mark as end of word
        node.isEndOfWord = true;
    }

    search(word) {
        let node = this.root;
        for (let char of word) {
            if (!node.has(char)) return false;
            node = node.get(char);
        }
        return node.isEndOfWord === true;
    }

    startsWith(prefix) {
        let node = this.root;
        for (let char of prefix) {
            if (!node.has(char)) return false;
            node = node.get(char);
        }
        return true;
    }
}
```

### Complexity:
- **Time Complexity:**
  - `insert`: O(n), where n is the length of the word.
  - `search`: O(n), where n is the length of the word.
  - `startsWith`: O(m), where m is the length of the prefix.
- **Space Complexity:** O(N*M), where N is the number of words inserted, and M is the average length of each word.

In conclusion, the Trie data structure efficiently supports insertions and lookup operations for dynamic sets of strings. By using maps, we enhance readability and slightly improve performance in scenarios with large and diverse sets of strings.

