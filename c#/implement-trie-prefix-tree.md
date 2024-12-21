# [Leetcode 208: Implement Trie (Prefix Tree)](https://leetcode.com/problems/implement-trie-prefix-tree/)

## Approaches
1. [Basic Approach: Using a List of Children](#basic-approach-using-a-list-of-children)
2. [Optimal Approach: Using a Dictionary of Children](#optimal-approach-using-a-dictionary-of-children)

### Basic Approach: Using a List of Children

#### Intuition

A Trie (Prefix Tree) is used to efficiently store and retrieve keys in a dataset of strings. We can implement a basic Trie using nodes, where each node contains an array (or list) of its children nodes. Each node will represent a character, and we'll mark the end of a word with a Boolean flag `isEnd`.

#### Code

```csharp
public class TrieNode {
    public TrieNode[] Children = new TrieNode[26];
    public bool IsEndOfWord = false;
}

public class Trie {
    private readonly TrieNode root;

    public Trie() {
        root = new TrieNode();
    }
    
    // Inserts a word into the trie.
    public void Insert(string word) {
        TrieNode node = root;
        foreach (char ch in word) {
            int index = ch - 'a'; // Calculate index based on character
            if (node.Children[index] == null) {
                node.Children[index] = new TrieNode(); // Create node if not found
            }
            node = node.Children[index]; // Move to the next node
        }
        node.IsEndOfWord = true; // Mark the end of the word
    }
    
    // Returns if the word is in the trie.
    public bool Search(string word) {
        TrieNode node = root;
        foreach (char ch in word) {
            int index = ch - 'a';
            if (node.Children[index] == null) {
                return false; // If child is missing, word is not present
            }
            node = node.Children[index];
        }
        return node.IsEndOfWord; // Check if we are at the end of the word
    }
    
    // Returns if there is any word in the trie that starts with the given prefix.
    public bool StartsWith(string prefix) {
        TrieNode node = root;
        foreach (char ch in prefix) {
            int index = ch - 'a';
            if (node.Children[index] == null) {
                return false; // Same logic as search for prefix
            }
            node = node.Children[index];
        }
        return true; // If we traverse the prefix successfully, return true
    }
}
```

#### Time Complexity

- Insert: O(L), where L is the length of the word. We traverse each character once.
- Search: O(L), similar reasoning as above.
- StartsWith: O(L), same as search and insert.

#### Space Complexity

- The worst-case space complexity is O(N * M), where N is the number of words and M is the average length of words. Each node can have up to 26 children (for English alphabets).

### Optimal Approach: Using a Dictionary of Children

#### Intuition

The above implementation can be improved using a dictionary to store child nodes. This not only accommodates words with different characters (in case of larger character sets) but also uses space more efficiently when a limited set of nodes exist.

#### Code

```csharp
public class TrieNode {
    public Dictionary<char, TrieNode> Children = new Dictionary<char, TrieNode>();
    public bool IsEndOfWord = false;
}

public class Trie {
    private readonly TrieNode root;

    public Trie() {
        root = new TrieNode();
    }
    
    // Inserts a word into the trie.
    public void Insert(string word) {
        TrieNode node = root;
        foreach (char ch in word) {
            if (!node.Children.ContainsKey(ch)) {
                node.Children[ch] = new TrieNode(); // Using dictionary to add new node
            }
            node = node.Children[ch]; // Move to the next node
        }
        node.IsEndOfWord = true; // Mark the end of the word
    }
    
    // Returns if the word is in the trie.
    public bool Search(string word) {
        TrieNode node = root;
        foreach (char ch in word) {
            if (!node.Children.ContainsKey(ch)) {
                return false; // If the character is missing, the word is not present
            }
            node = node.Children[ch];
        }
        return node.IsEndOfWord; // Check if we are at the end of the word
    }
    
    // Returns if there is any word in the trie that starts with the given prefix.
    public bool StartsWith(string prefix) {
        TrieNode node = root;
        foreach (char ch in prefix) {
            if (!node.Children.ContainsKey(ch)) {
                return false; // If character is missing, prefix does not exist
            }
            node = node.Children[ch];
        }
        return true; // If we traverse the prefix successfully, return true
    }
}
```

#### Time Complexity

- Insert: O(L), where L is the length of the word, as we perform a simple walkthrough.
- Search: O(L), same complexity reason as insert.
- StartsWith: O(L), similar as above.

#### Space Complexity

- More efficient space usage with O(N * M) in the worst case, but typically less due to utilizing only required nodes with dictionaries.

