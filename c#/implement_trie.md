# 208. [Implement Trie (Prefix Tree)](https://leetcode.com/problems/implement-trie-prefix-tree/)

## Approach: Trie Node Implementation

### Solution
```csharp
// Time Complexity:
//   - Insert(): O(m), where m is the length of the word
//   - Search(): O(m), where m is the length of the word
//   - StartsWith(): O(m), where m is the length of the prefix
// Space Complexity: O(n * m), where n is the number of words and m is the average word length

public class Trie {

    private TrieNode root;

    public Trie() {
        root = new TrieNode(); // Initialize the root node
    }

    // Inserts a word into the trie
    public void Insert(string word) {
        TrieNode node = root;
        foreach (char c in word.ToCharArray()) {
            if (!node.ContainsKey(c)) {
                node.Put(c, new TrieNode()); // Create a new node if not already present
            }
            node = node.Get(c); // Move to the next node
        }
        node.SetEnd(); // Mark the end of the word
    }

    // Returns true if the word is in the trie
    public bool Search(string word) {
        TrieNode node = SearchPrefix(word);
        return node != null && node.IsEnd(); // Check if the word ends at this node
    }

    // Returns true if there is any word in the trie that starts with the given prefix
    public bool StartsWith(string prefix) {
        return SearchPrefix(prefix) != null;
    }

    // Helper function to search for a node matching the prefix
    private TrieNode SearchPrefix(string prefix) {
        TrieNode node = root;
        foreach (char c in prefix.ToCharArray()) {
            if (node.ContainsKey(c)) {
                node = node.Get(c); // Move to the next node
            } else {
                return null; // Prefix not found
            }
        }
        return node; // Return the last node of the prefix
    }
}

// Trie Node Class
public class TrieNode {
    private TrieNode[] links;
    private const int R = 26; // Number of possible children ('a' to 'z')
    private bool isEnd;

    public TrieNode() {
        links = new TrieNode[R];
    }

    public bool ContainsKey(char c) {
        return links[c - 'a'] != null;
    }

    public TrieNode Get(char c) {
        return links[c - 'a'];
    }

    public void Put(char c, TrieNode node) {
        links[c - 'a'] = node;
    }

    public void SetEnd() {
        isEnd = true;
    }

    public bool IsEnd() {
        return isEnd;
    }
}
```

