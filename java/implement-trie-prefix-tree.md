# [Leetcode 208: Implement Trie (Prefix Tree)](https://leetcode.com/problems/implement-trie-prefix-tree/)

## Approaches:

- [Approach 1: Basic Trie Implementation](#approach-1-basic-trie-implementation)
- [Approach 2: Optimized Trie Implementation with HashMap](#approach-2-optimized-trie-implementation-with-hashmap)

## Approach 1: Basic Trie Implementation

### Intuition

The basic idea of a Trie is to store strings in a tree-like structure where each node represents a single character. Each path down the tree represents a string. We add two special features to our nodes:
1. A way to know if they represent the end of a valid string.
2. Pointers to traverse to the next node based on the next character.

We'll implement the basic Trie operations:
- **Insert**: Traverse character by character, creating nodes if they don't exist.
- **Search**: Traverse character by character to see if the whole word exists.
- **StartsWith**: Similar to search, but only checks if the prefix exists.

### Java Code

```java
class Trie {

    private static class TrieNode {
        TrieNode[] children;
        boolean isEndOfWord;

        public TrieNode() {
            // Initialize the children array for 26 alphabets
            children = new TrieNode[26];
            isEndOfWord = false;
        }
    }

    private final TrieNode root;

    public Trie() {
        root = new TrieNode();
    }

    public void insert(String word) {
        TrieNode current = root;
        for (char ch : word.toCharArray()) {
            int index = ch - 'a'; // Calculate the index for character
            if (current.children[index] == null) {
                current.children[index] = new TrieNode(); // Create child if not present
            }
            current = current.children[index];
        }
        current.isEndOfWord = true; // Mark the end of a word
    }

    public boolean search(String word) {
        TrieNode current = root;
        for (char ch : word.toCharArray()) {
            int index = ch - 'a';
            if (current.children[index] == null) {
                return false; // Character path doesn't exist, hence word not present
            }
            current = current.children[index];
        }
        return current.isEndOfWord; // Return true if it's the end of a word
    }

    public boolean startsWith(String prefix) {
        TrieNode current = root;
        for (char ch : prefix.toCharArray()) {
            int index = ch - 'a';
            if (current.children[index] == null) {
                return false; // Prefix path doesn't exist
            }
            current = current.children[index];
        }
        return true; // Prefix exists
    }
}
```

### Time and Space Complexity

- **Insert**: O(n), where n is the length of the word to insert.
- **Search/StartsWith**: O(n), where n is the length of the word/prefix to search.
- **Space Complexity**: O(n * m), where n is the number of words and m is the length of the longest word.

## Approach 2: Optimized Trie Implementation with HashMap

### Intuition

Instead of using a fixed size array for the children nodes, we can use a HashMap to dynamically store only the necessary characters. This saves space, especially in cases where the alphabet size is large or the set of possible characters is sparse.

### Java Code

```java
import java.util.HashMap;
import java.util.Map;

class Trie {

    private static class TrieNode {
        Map<Character, TrieNode> children;
        boolean isEndOfWord;

        public TrieNode() {
            children = new HashMap<>(); // Use a hash map for dynamic children nodes
            isEndOfWord = false;
        }
    }

    private final TrieNode root;

    public Trie() {
        root = new TrieNode();
    }

    public void insert(String word) {
        TrieNode current = root;
        for (char ch : word.toCharArray()) {
            current.children.putIfAbsent(ch, new TrieNode()); // Add node if absent
            current = current.children.get(ch);
        }
        current.isEndOfWord = true; // Mark the end of a word
    }

    public boolean search(String word) {
        TrieNode current = root;
        for (char ch : word.toCharArray()) {
            current = current.children.get(ch);
            if (current == null) {
                return false; // Node doesn't exist for character, hence word not present
            }
        }
        return current.isEndOfWord; // Return true if it's end of the word
    }

    public boolean startsWith(String prefix) {
        TrieNode current = root;
        for (char ch : prefix.toCharArray()) {
            current = current.children.get(ch);
            if (current == null) {
                return false; // Node doesn't exist for character, hence prefix not present
            }
        }
        return true; // Prefix exists
    }
}
```

### Time and Space Complexity

- **Insert**: O(n), where n is the length of the word to insert.
- **Search/StartsWith**: O(n), where n is the length of the word/prefix to search.
- **Space Complexity**: In practice, it can be lower than using a fixed array due to dynamic allocation of children, especially if the set of possible characters is sparse.

