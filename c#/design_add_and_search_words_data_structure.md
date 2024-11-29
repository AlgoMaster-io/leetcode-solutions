# 211. [Design Add and Search Words Data Structure](https://leetcode.com/problems/design-add-and-search-words-data-structure/)

## Approach: Trie with Backtracking for Search

### Solution
csharp
```csharp
// Time Complexity:
//   - AddWord(): O(m), where m is the length of the word
//   - Search(): O(m * 26^k), where m is the length of the word and k is the number of wildcards
// Space Complexity: O(n * m), where n is the number of words and m is the average word length

public class WordDictionary {

    private class TrieNode {
        public TrieNode[] Links { get; private set; }
        private const int R = 26; // Number of possible children ('a' to 'z')
        public bool IsEnd { get; private set; }

        public TrieNode() {
            Links = new TrieNode[R];
        }

        public bool ContainsKey(char c) {
            return Links[c - 'a'] != null;
        }

        public TrieNode Get(char c) {
            return Links[c - 'a'];
        }

        public void Put(char c, TrieNode node) {
            Links[c - 'a'] = node;
        }

        public void SetEnd() {
            IsEnd = true;
        }
    }

    private TrieNode root;

    public WordDictionary() {
        root = new TrieNode(); // Initialize the root node
    }

    // Adds a word into the data structure
    public void AddWord(string word) {
        TrieNode node = root;
        foreach (char c in word.ToCharArray()) {
            if (!node.ContainsKey(c)) {
                node.Put(c, new TrieNode()); // Create a new node if not already present
            }
            node = node.Get(c); // Move to the next node
        }
        node.SetEnd(); // Mark the end of the word
    }

    // Returns true if the word is in the data structure, allowing '.' as a wildcard
    public bool Search(string word) {
        return SearchInNode(word, 0, root);
    }

    // Helper function for backtracking search
    private bool SearchInNode(string word, int index, TrieNode node) {
        if (node == null) {
            return false; // Base case: node doesn't exist
        }
        if (index == word.Length) {
            return node.IsEnd; // Check if the word ends at this node
        }

        char c = word[index];
        if (c == '.') {
            // If the current character is '.', check all possible children
            for (char nextChar = 'a'; nextChar <= 'z'; nextChar++) {
                if (SearchInNode(word, index + 1, node.Get(nextChar))) {
                    return true;
                }
            }
        } else {
            // If the current character is not '.', proceed with the specific child
            return SearchInNode(word, index + 1, node.Get(c));
        }
        return false; // No match found
    }
}
```

