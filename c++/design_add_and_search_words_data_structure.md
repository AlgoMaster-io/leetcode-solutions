# 211. [Design Add and Search Words Data Structure](https://leetcode.com/problems/design-add-and-search-words-data-structure/)

## Approach: Trie with Backtracking for Search

### Solution
```cpp
// Time Complexity:
//   - addWord(): O(m), where m is the length of the word
//   - search(): O(m * 26^k), where m is the length of the word and k is the number of wildcards
// Space Complexity: O(n * m), where n is the number of words and m is the average word length

#include <vector>
#include <string>

class TrieNode {
private:
    std::vector<TrieNode*> links;
    static const int R = 26;
    bool isEnd;
    
public:
    TrieNode() : links(R, nullptr), isEnd(false) {}

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

    bool isEndOfWord() {
        return isEnd;
    }
};

class WordDictionary {
private:
    TrieNode* root;

    bool searchInNode(const std::string& word, int index, TrieNode* node) {
        if (node == nullptr) {
            return false;
        }
        if (index == word.length()) {
            return node->isEndOfWord();
        }

        char c = word[index];
        if (c == '.') {
            for (char nextChar = 'a'; nextChar <= 'z'; nextChar++) {
                if (searchInNode(word, index + 1, node->get(nextChar))) {
                    return true;
                }
            }
        } else {
            return searchInNode(word, index + 1, node->get(c));
        }
        return false;
    }
    
public:
    WordDictionary() {
        root = new TrieNode();
    }

    void addWord(std::string word) {
        TrieNode* node = root;
        for (char c : word) {
            if (!node->containsKey(c)) {
                node->put(c, new TrieNode());
            }
            node = node->get(c);
        }
        node->setEnd();
    }

    bool search(std::string word) {
        return searchInNode(word, 0, root);
    }
};
```

