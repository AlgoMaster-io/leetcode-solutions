# [Leetcode 212: Word Search II](https://leetcode.com/problems/word-search-ii/)

## Approaches

1. [Backtracking with Set](#backtracking-with-set)
2. [Trie with Backtracking](#trie-with-backtracking)

## Approach 1: Backtracking with Set

### Intuition

The Word Search II problem involves finding all the words from the dictionary that appear on a given board. A simple approach is to perform a backtracking search (DFS) for each word on the board. We will traverse the board and initiate a DFS whenever the first character of the word matches. If the entire word can be traced from any starting point, we add the word to our result.

### Steps:
- Iterate over the board and for each cell, use DFS to try to construct each word from the dictionary.
- Check bounds for DFS and ensure no cells are visited twice for the same word search.
- Use a set to collect found words to avoid duplicates.

### Solution

```cpp
class Solution {
public:
    vector<string> findWords(vector<vector<char>>& board, vector<string>& words) {
        unordered_set<string> result;
        for (string& word : words) {
            if (exist(board, word)) {
                result.insert(word);
            }
        }
        return vector<string>(result.begin(), result.end());
    }
    
private:
    bool exist(vector<vector<char>>& board, const string& word) {
        for (int i = 0; i < board.size(); ++i) {
            for (int j = 0; j < board[0].size(); ++j) {
                if (dfs(board, i, j, 0, word)) {
                    return true;
                }
            }
        }
        return false;
    }

    bool dfs(vector<vector<char>>& board, int i, int j, int k, const string& word) {
        if (k == word.length()) return true; // Whole word found

        if (i < 0 || i >= board.size() || j < 0 || j >= board[0].size() || board[i][j] != word[k]) {
            return false; // Out of bounds or mismatch
        }
        
        char tmp = board[i][j]; // Store current character
        board[i][j] = '#'; // Mark as visited
        bool found = dfs(board, i + 1, j, k + 1, word) ||
                     dfs(board, i - 1, j, k + 1, word) ||
                     dfs(board, i, j + 1, k + 1, word) ||
                     dfs(board, i, j - 1, k + 1, word);
        board[i][j] = tmp; // Restore character
        
        return found;
    }
};
```

### Complexity Analysis
- **Time Complexity**: O(N \* 3^L \* W), where N is the number of cells on the board, L is the maximum length of a word, and W is the total number of words in the dictionary.
- **Space Complexity**: O(L), for recursion stack maximum depth.

## Approach 2: Trie with Backtracking

### Intuition

By using a Trie to store all the words, we can significantly optimize the searching process. The Trie allows us to efficiently search for possible words by cutting down unnecessary paths early. During the DFS, if the current path does not represent any prefix in the Trie, we return early.

### Steps:
1. Build a Trie from the list of words.
2. Use DFS to explore the board.
3. At each cell, if the character exists in the current Trie node, continue searching.
4. If a word is found, add it to the result set to avoid duplicates.
5. Ensure to handle backtracking by marking cells as visited temporarily.

### Solution

```cpp
class TrieNode {
public:
    unordered_map<char, TrieNode*> children;
    string word;
    
    TrieNode() : word("") {}
};

class Solution {
public:
    vector<string> findWords(vector<vector<char>>& board, vector<string>& words) {
        TrieNode* root = buildTrie(words);
        vector<string> result;
        
        for (int i = 0; i < board.size(); ++i) {
            for (int j = 0; j < board[0].size(); ++j) {
                dfs(board, i, j, root, result);
            }
        }
        
        return result;
    }
    
private:
    TrieNode* buildTrie(vector<string>& words) {
        TrieNode* root = new TrieNode();
        for (string& word : words) {
            TrieNode* node = root;
            for (char c : word) {
                if (!node->children.count(c)) {
                    node->children[c] = new TrieNode();
                }
                node = node->children[c];
            }
            node->word = word;
        }
        return root;
    }
    
    void dfs(vector<vector<char>>& board, int i, int j, TrieNode* node, vector<string>& result) {
        char c = board[i][j];
        if (c == '#' || !node->children.count(c)) return;
        node = node->children[c];
        if (!node->word.empty()) {
            result.push_back(node->word);
            node->word.clear(); // Avoid duplicates
        }
        
        board[i][j] = '#'; // Mark as visited
        if (i > 0) dfs(board, i - 1, j, node, result);
        if (j > 0) dfs(board, i, j - 1, node, result);
        if (i < board.size() - 1) dfs(board, i + 1, j, node, result);
        if (j < board[0].size() - 1) dfs(board, i, j + 1, node, result);
        board[i][j] = c; // Restore character
    }
};

```

### Complexity Analysis
- **Time Complexity**: O(N \* 3^L), where N is the number of cells on the board, and L is the maximum length of a word in the Trie. The Trie reduces redundant searches.
- **Space Complexity**: O(W \* L), where W is the number of words, and L is the length of the longest word for the Trie storage.

