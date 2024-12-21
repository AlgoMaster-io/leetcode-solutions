[Leetcode Problem 211: Design Add and Search Words Data Structure](https://leetcode.com/problems/design-add-and-search-words-data-structure/)

### Approaches:
- [Approach 1: Brute Force with Linear Search](#approach-1-brute-force-with-linear-search)
- [Approach 2: Trie Data Structure](#approach-2-trie-data-structure)

---

### Approach 1: Brute Force with Linear Search

#### Intuition:
The simplest way to solve this problem is by maintaining a list of words and checking each word one by one when searching. The `addWord` function would add words to the list, and the `search` function would iterate over the list to check for matches. When searching, if the word contains a '.', we'll check if there is at least one matching word by trying to match each character specifically.

#### Code:
```cpp
#include <vector>
#include <string>
using namespace std;

class WordDictionary {
public:
    vector<string> words;

    WordDictionary() { }

    // Add a word to the list
    void addWord(string word) {
        words.push_back(word);
    }

    // Search each word from list linearly
    bool search(string word) {
        for (const string &w : words) {
            if (w.length() != word.length()) continue; // only consider words of same length

            bool match = true;
            for (int i = 0; i < word.length(); i++) {
                if (word[i] != '.' && w[i] != word[i]) {
                    match = false;
                    break;
                }
            }
            if (match) return true;
        }
        return false;
    }
};
```

#### Complexity Analysis:
- **Time Complexity**: 
  - `addWord`: \(O(1)\), as we simply add the word to the list.
  - `search`: \(O(n \cdot m)\), where \(n\) is the number of words in the dictionary and \(m\) is the average word length.
- **Space Complexity**: 
  - \(O(n \cdot m)\) for storing all words in the list.

---

### Approach 2: Trie Data Structure

#### Intuition:
A better approach is to use a Trie (prefix tree) for storing the words. A Trie is well-suited for search operations and handles wildcards naturally. Each node in the Trie will represent a character of a word, and special logic is used to handle the '.' wildcard during search.

#### Code:
```cpp
#include <unordered_map>
#include <string>
using namespace std;

class TrieNode {
public:
    unordered_map<char, TrieNode*> children;
    bool isEndOfWord;

    TrieNode() : isEndOfWord(false) {}
};

class WordDictionary {
private:
    TrieNode* root;

public:
    WordDictionary() {
        root = new TrieNode();
    }

    // Inserts a word into the trie
    void addWord(string word) {
        TrieNode* current = root;
        for (char &ch : word) {
            if (!current->children.count(ch)) {
                current->children[ch] = new TrieNode();
            }
            current = current->children[ch];
        }
        current->isEndOfWord = true;
    }
    
    // Main search function
    bool search(string word) {
        return searchInNode(word, 0, root);
    }
    
private:
    // Recursive function to search within the Trie
    bool searchInNode(const string &word, int index, TrieNode* node) {
        if (index == word.length()) {
            return node->isEndOfWord;
        }
        
        char ch = word[index];
        if (ch == '.') {
            for (auto &pair : node->children) {
                if (searchInNode(word, index + 1, pair.second)) {
                    return true;
                }
            }
            return false;
        } else {
            if (!node->children.count(ch)) {
                return false;
            }
            return searchInNode(word, index + 1, node->children[ch]);
        }
    }
};
```

#### Complexity Analysis:
- **Time Complexity**:
  - `addWord`: \(O(m)\), where \(m\) is the length of the word.
  - `search`: In the worst-case scenario, it can become \(O(n \cdot m)\) because of backtracking in the case of multiple wildcards.
- **Space Complexity**: 
  - \(O(n \cdot m)\) for storing all nodes in the trie, where \(n\) is the number of words and \(m\) is the average length of the words.

This approach is much more efficient for incremental build-up and searches, particularly benefiting from prefix-based optimizations.

