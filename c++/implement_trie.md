# 208. [Implement Trie (Prefix Tree)](https://leetcode.com/problems/implement-trie-prefix-tree/)

## Approach: Trie Node Implementation

### Solution
```cpp
// Time Complexity:
//   - insert(): O(m), where m is the length of the word
//   - search(): O(m), where m is the length of the word
//   - startsWith(): O(m), where m is the length of the prefix
// Space Complexity: O(n * m), where n is the number of words and m is the average word length

class TrieNode {
private:
    TrieNode* links[26];
    bool isEnd = false;

public:
    TrieNode() {
        // Initialize links to null
        for (int i = 0; i < 26; ++i) {
            links[i] = nullptr;
        }
    }

    bool containsKey(char c) {
        return links[c - 'a'] != nullptr;
    }

    TrieNode* get(char c) {
        return links[c - 'a'];
    }

    void put(char c, TrieNode* node) {
        links[c - 'a'] = node;
    }

    void setEnd() {
        isEnd = true;
    }

    bool isEndNode() {
        return isEnd;
    }
};

class Trie {
private:
    TrieNode* root;

    TrieNode* searchPrefix(const std::string& prefix) {
        TrieNode* node = root;
        for (char c : prefix) {
            if (node->containsKey(c)) {
                node = node->get(c); // Move to the next node
            } else {
                return nullptr; // Prefix not found
            }
        }
        return node; // Return the last node of the prefix
    }

public:
    Trie() {
        root = new TrieNode(); // Initialize the root node
    }

    // Inserts a word into the trie
    void insert(const std::string& word) {
        TrieNode* node = root;
        for (char c : word) {
            if (!node->containsKey(c)) {
                node->put(c, new TrieNode()); // Create a new node if not already present
            }
            node = node->get(c); // Move to the next node
        }
        node->setEnd(); // Mark the end of the word
    }

    // Returns true if the word is in the trie
    bool search(const std::string& word) {
        TrieNode* node = searchPrefix(word);
        return node != nullptr && node->isEndNode(); // Check if the word ends at this node
    }

    // Returns true if there is any word in the trie that starts with the given prefix
    bool startsWith(const std::string& prefix) {
        return searchPrefix(prefix) != nullptr;
    }
};
```

