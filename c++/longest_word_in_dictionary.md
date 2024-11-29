# 720. [Longest Word in Dictionary](https://leetcode.com/problems/longest-word-in-dictionary/)

## Approach 1: Sort and HashSet

### Solution
```cpp
// Time Complexity: O(n * k), where n is the number of words and k is the average length of a word
// Space Complexity: O(n * k)
#include <algorithm>
#include <unordered_set>
#include <string>
#include <vector>

class Solution {
public:
    std::string longestWord(std::vector<std::string>& words) {
        std::sort(words.begin(), words.end()); // Sort words lexicographically
        std::unordered_set<std::string> built;
        std::string result = "";

        for (const std::string& word : words) {
            // Check if the prefix (word without the last character) is in the set
            if (word.length() == 1 || built.count(word.substr(0, word.length() - 1))) {
                built.insert(word); // Add the word to the set
                // Update result if the word is longer or lexicographically smaller
                if (word.length() > result.length() || 
                    (word.length() == result.length() && word < result)) {
                    result = word;
                }
            }
        }

        return result;
    }
};
```

## Approach 2: Trie-Based Solution

### Solution
```cpp
// Time Complexity: O(n * k), where n is the number of words and k is the average length of a word
// Space Complexity: O(n * k)
#include <unordered_map>
#include <string>
#include <vector>

class TrieNode {
public:
    std::unordered_map<char, TrieNode*> children;
    bool isEndOfWord = false;
    std::string word = "";
};

class Solution {
public:
    std::string longestWord(std::vector<std::string>& words) {
        TrieNode* root = new TrieNode();

        // Build the Trie
        for (const std::string& word : words) {
            TrieNode* current = root;
            for (char c : word) {
                if (current->children.find(c) == current->children.end()) {
                    current->children[c] = new TrieNode();
                }
                current = current->children[c];
            }
            current->isEndOfWord = true;
            current->word = word;
        }

        // Perform DFS to find the longest word
        return dfs(root, "");
    }

private:
    std::string dfs(TrieNode* node, std::string result) {
        if (node == nullptr || (!node->isEndOfWord && node != nullptr)) {
            return result;
        }

        // Check for the longest word in the current branch
        if (node->word.length() > result.length() || 
            (node->word.length() == result.length() && node->word < result)) {
            result = node->word;
        }

        for (char c = 'a'; c <= 'z'; c++) {
            result = dfs(node->children[c], result);
        }

        return result;
    }
};
```

