# [Leetcode 208: Implement Trie (Prefix Tree)](https://leetcode.com/problems/implement-trie-prefix-tree/)

## Table of Contents
- [Approach 1: Using an Array of Pointers](#approach-1-using-an-array-of-pointers)
- [Complexity Analysis](#complexity-analysis)

## Approach 1: Using an Array of Pointers

### Intuition
A Trie, also known as a prefix tree, is a special type of tree used to store associative data structures. The key idea in a Trie is to store common prefixes of strings efficiently by sharing them in the nodes. We need an efficient data structure to support the following operations:
- Inserting a word into the Trie (`insert`).
- Searching if a word is present in the Trie (`search`).
- Checking if there is any word in the Trie that starts with a given prefix (`startsWith`).

The approach we're going to use involves creating a TrieNode class to represent each node of the Trie. Each node will have an array of pointers representing the next possible characters (considering the 26 lowercase English letters, 'a' to 'z') and a boolean flag to mark the end of a word.

### Implementation

```cpp
class TrieNode {
public:
    TrieNode* children[26];
    bool isEndOfWord;

    TrieNode() {
        // Initialize all children as null and isEndOfWord as false
        for (int i = 0; i < 26; ++i) {
            children[i] = nullptr;
        }
        isEndOfWord = false;
    }
};

class Trie {
    TrieNode* root;

public:
    /** Initialize your data structure here. */
    Trie() {
        root = new TrieNode();
    }
    
    /** Inserts a word into the trie. */
    void insert(string word) {
        TrieNode* node = root;
        for (char ch : word) {
            int index = ch - 'a';  // Convert character to index
            if (!node->children[index]) {
                node->children[index] = new TrieNode();
            }
            node = node->children[index];
        }
        node->isEndOfWord = true;  // Mark the last node as end of a word
    }
    
    /** Returns if the word is in the trie. */
    bool search(string word) {
        TrieNode* node = root;
        for (char ch : word) {
            int index = ch - 'a';
            if (!node->children[index]) {
                return false;  // Return false if character path does not exist
            }
            node = node->children[index];
        }
        return node->isEndOfWord;  // Check if end of word is reached
    }
    
    /** Returns if there is any word in the trie that starts with the given prefix. */
    bool startsWith(string prefix) {
        TrieNode* node = root;
        for (char ch : prefix) {
            int index = ch - 'a';
            if (!node->children[index]) {
                return false;  // Return false if prefix path does not exist
            }
            node = node->children[index];
        }
        return true;  // Return true if prefix exists
    }
};
```

### Complexity Analysis

- **Time Complexity**:
  - `insert`: O(m), where m is the length of the word being inserted. We need to traverse through the word and insert nodes if they don’t exist yet.
  - `search`: O(m), where m is the length of the word being searched. We traverse the Trie following the path dictated by the string.
  - `startsWith`: O(m), where m is the length of the prefix. It's similar to search, but we don't have to check if it is the end of a word.

- **Space Complexity**: O(N * M * 26), where N is the number of words inserted in the Trie and M is the average length of these words. Each node could potentially branch into 26 different nodes for each character in the English alphabet (a-z).

This approach provides a solid and straightforward way to implement and utilize a Trie, with acceptable performance for a wide variety of applications involving prefix searches.

