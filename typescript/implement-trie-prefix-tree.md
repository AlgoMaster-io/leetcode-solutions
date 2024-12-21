# [Leetcode 208: Implement Trie (Prefix Tree)](https://leetcode.com/problems/implement-trie-prefix-tree/)

## Table of Contents
- [Approach 1: Basic Implementation Using a Tree](#approach-1)
- [Approach 2: Improved Implementation Using HashMap](#approach-2)
  
### Approach 1: Basic Implementation Using a Tree

#### Intuition
The Trie, also known as a prefix tree, is a multi-way tree structure useful for storing strings over an alphabet. Each node in the Trie represents a single character. All descendants of a node have a common prefix of the string associated with that node. For a particular word, the last node should mark the end of that word.

In this basic approach, we'll use a simple tree structure where each node can have up to 26 children (one for each letter with lowercase English alphabets). Here's the simple way to design it:

#### Implementation

```typescript
class TrieNode {
  children: TrieNode[];
  isEndOfWord: boolean;

  constructor() {
    this.children = new Array(26).fill(null);
    this.isEndOfWord = false;
  }
}

class Trie {
  root: TrieNode;

  constructor() {
    this.root = new TrieNode();
  }

  // Inserts a word into the trie.
  insert(word: string): void {
    let currentNode = this.root;
    for (let char of word) {
      const index = char.charCodeAt(0) - 'a'.charCodeAt(0); // Compute index for the character
      if (!currentNode.children[index]) {
        currentNode.children[index] = new TrieNode(); // Create a new node if necessary
      }
      currentNode = currentNode.children[index];
    }
    currentNode.isEndOfWord = true; // Mark the end of a word
  }

  // Returns true if the word is in the trie.
  search(word: string): boolean {
    let currentNode = this.root;
    for (let char of word) {
      const index = char.charCodeAt(0) - 'a'.charCodeAt(0);
      if (!currentNode.children[index]) {
        return false; // Return false if the character path doesn't exist
      }
      currentNode = currentNode.children[index];
    }
    return currentNode.isEndOfWord; // Return true if current node marks end of word
  }

  // Returns true if there is any word in the trie that starts with the given prefix.
  startsWith(prefix: string): boolean {
    let currentNode = this.root;
    for (let char of prefix) {
      const index = char.charCodeAt(0) - 'a'.charCodeAt(0);
      if (!currentNode.children[index]) {
        return false; // Return false if the prefix path doesn't exist
      }
      currentNode = currentNode.children[index];
    }
    return true; // Prefix exists in the Trie
  }
}
```

#### Complexity
- Time Complexity: Inserting and searching a word both take O(n) time, where n is the length of the word.
- Space Complexity: In the worst case, storing n words with average length m in the trie takes O(m * n) space.

### Approach 2: Improved Implementation Using HashMap

#### Intuition
The previous approach uses an array of fixed size 26 for storing children references. This can be made more efficient by using a Map or object in JavaScript/TypeScript to only store the necessary child references for each node.

This reduces the space consumption, especially in situations where the branching factor is small. The trade-off is slightly increased time in inserting and searching due to map operations, but it is generally faster due to reduced memory usage.

#### Implementation

```typescript
class ImprovedTrieNode {
  children: Map<string, ImprovedTrieNode>;
  isEndOfWord: boolean;

  constructor() {
    this.children = new Map<string, ImprovedTrieNode>();
    this.isEndOfWord = false;
  }
}

class ImprovedTrie {
  root: ImprovedTrieNode;

  constructor() {
    this.root = new ImprovedTrieNode();
  }

  // Inserts a word into the trie.
  insert(word: string): void {
    let currentNode = this.root;
    for (let char of word) {
      if (!currentNode.children.has(char)) {
        currentNode.children.set(char, new ImprovedTrieNode()); // Use a map to store the next node
      }
      currentNode = currentNode.children.get(char)!;
    }
    currentNode.isEndOfWord = true;
  }

  // Returns true if the word is in the trie.
  search(word: string): boolean {
    let currentNode = this.root;
    for (let char of word) {
      if (!currentNode.children.has(char)) {
        return false; // Return false if the character path doesn't exist
      }
      currentNode = currentNode.children.get(char)!;
    }
    return currentNode.isEndOfWord;
  }

  // Returns true if there is any word in the trie that starts with the given prefix.
  startsWith(prefix: string): boolean {
    let currentNode = this.root;
    for (let char of prefix) {
      if (!currentNode.children.has(char)) {
        return false;
      }
      currentNode = currentNode.children.get(char)!;
    }
    return true;
  }
}
```

#### Complexity
- Time Complexity: Insertion and searching take O(n) time, where n is the length of the word.
- Space Complexity: Save further space compared to the array approach, especially for sparse trie structures. 

Using this improved approach or the basic one depends on the specific use case and system constraints regarding time and space efficiency.

