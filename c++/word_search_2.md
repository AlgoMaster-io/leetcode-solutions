# 212. [Word Search II](https://leetcode.com/problems/word-search-ii/)

## Approach 1: Trie + Backtracking

### Solution
```cpp
// Time Complexity: O(n * m * 4^l), where n and m are the dimensions of the board, and l is the average word length
// Space Complexity: O(w * l), where w is the number of words and l is the average length of a word in the dictionary
#include <vector>
#include <string>
#include <unordered_map>

using namespace std;

class TrieNode {
public:
    unordered_map<char, TrieNode*> children;
    string word = ""; // Store the word ending at this node
};

class Solution {
public:
    vector<string> findWords(vector<vector<char>>& board, vector<string>& words) {
        // Build the Trie
        TrieNode* root = new TrieNode();
        for (string word : words) {
            TrieNode* current = root;
            for (char c : word) {
                if (current->children.find(c) == current->children.end()) 
                    current->children[c] = new TrieNode();
                current = current->children[c];
            }
            current->word = word; // Mark the end of a word
        }

        vector<string> result;
        for (int i = 0; i < board.size(); i++) {
            for (int j = 0; j < board[0].size(); j++) {
                backtrack(board, i, j, root, result);
            }
        }
        return result;
    }

private:
    void backtrack(vector<vector<char>>& board, int row, int col, TrieNode* node, vector<string>& result) {
        char letter = board[row][col];
        TrieNode* current = node->children[letter];

        // If no Trie node exists, return
        if (!current) return;

        // Check if the current node contains a word
        if (!current->word.empty()) {
            result.push_back(current->word);
            current->word = ""; // Avoid duplicate entries
        }

        // Mark the current cell as visited
        board[row][col] = '#';

        // Explore the neighbors
        int rowOffsets[] = {-1, 1, 0, 0};
        int colOffsets[] = {0, 0, -1, 1};
        for (int i = 0; i < 4; i++) {
            int newRow = row + rowOffsets[i];
            int newCol = col + colOffsets[i];
            if (newRow >= 0 && newRow < board.size() && newCol >= 0 && newCol < board[0].size()) {
                backtrack(board, newRow, newCol, current, result);
            }
        }

        // Restore the cell value
        board[row][col] = letter;

        // Optimization: Remove the leaf Trie node
        if (current->children.empty()) {
            node->children.erase(letter);
        }
    }
};
```

