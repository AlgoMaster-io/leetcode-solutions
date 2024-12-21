# [Leetcode 720: Longest Word in Dictionary](https://leetcode.com/problems/longest-word-in-dictionary/)

## Approaches:
- [Brute Force Approach](#brute-force-approach)
- [Trie Approach](#trie-approach)

---

### Brute Force Approach

#### Intuition:
The main intuition behind this approach is to use sorting and hash set features. The words need to be checked in an incremental order of their lengths. By sorting the words lexicographically, we can ensure that once a word can be formed, all its prefixes should be present, so we can add it to our hash set and continue building larger words from it.

#### Steps:
1. Sort the words array lexicographically to handle ties.
2. Initialize a hash set to store words that can be built one character at a time.
3. Traverse each word, and for each word, check if it can be formed by ensuring that its prefix (obtained by removing the last character of the word) is present in the hash set.
4. If a word can be formed, add it to the hash set and keep track of the longest formed word as the answer.
5. Return the longest word tracked.

```cpp
string longestWord(vector<string>& words) {
    sort(words.begin(), words.end()); // Step 1: Sort the words
    unordered_set<string> buildableWords;
    buildableWords.insert(""); // Start with empty string as buildable
    string longestWord = "";

    for (const string& word : words) {
        // If the prefix (word without the last char) is present in the buildable set
        if (buildableWords.count(word.substr(0, word.length() - 1))) {
            buildableWords.insert(word); // Add current word to buildable set

            // Update longest word if the current one is longer or comes first lexicographically
            if (word.length() > longestWord.length()) {
                longestWord = word;
            }
        }
    }
    return longestWord;
}
```

- **Time Complexity:** O(N log N + N * M), where N is the number of words and M is the maximum length of a word.
- **Space Complexity:** O(N * M), the space for storing words in a hash set.

---

### Trie Approach

#### Intuition:
A Trie, or prefix tree, can be highly effective here because it directly supports prefix operations. By inserting words into a trie, we can efficiently check if all prefixes of a word exist while tracking the longest word.

#### Steps:
1. Create a Trie data structure and insert all words into it.
2. During insertion, maintain each word's end position using Trie nodes.
3. Traverse the Trie starting from the root to check if a word can be rebuilt from the existing path.
4. Use a depth-first search (DFS) on the Trie to find the longest valid word constructible from prefixes.

```cpp
class TrieNode {
public:
    TrieNode* children[26];
    string word;
    TrieNode() {
        for (int i = 0; i < 26; ++i) {
            children[i] = nullptr;
        }
        word = "";
    }
};

class Trie {
public:
    TrieNode* root;
    Trie() {
        root = new TrieNode();
    }

    // Insert a word into the Trie
    void insert(string word) {
        TrieNode* node = root;
        for (char c : word) {
            int index = c - 'a';
            if (!node->children[index]) {
                node->children[index] = new TrieNode();
            }
            node = node->children[index];
        }
        node->word = word;
    }

    // Perform DFS to find the longest word with valid prefixes
    string findLongestWord() {
        return dfs(root, "");
    }

private:
    string dfs(TrieNode* node, string current) {
        string longestWord = node->word;
        for (TrieNode* child : node->children) {
            if (child && child->word != "") {
                string childWord = dfs(child, child->word);
                if (childWord.size() > longestWord.size() || 
                    (childWord.size() == longestWord.size() && childWord < longestWord)) {
                    longestWord = childWord;
                }
            }
        }
        return longestWord;
    }
};

string longestWord(vector<string>& words) {
    Trie trie;
    for (const string& word : words) {
        trie.insert(word);
    }
    return trie.findLongestWord();
}
```

- **Time Complexity:** O(N * M), where N is the number of words and M is the length of the longest word.
- **Space Complexity:** O(N * M), for storing the Trie and the words.

Concluding, we discussed two approaches to solve the problem: a brute force approach using sorting and hash set, and an optimized approach using Trie data structure. Each holds its strengths based on different priorities and constraints of the problem instance.

