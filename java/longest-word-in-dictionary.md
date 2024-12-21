# [Leetcode 720: Longest Word in Dictionary](https://leetcode.com/problems/longest-word-in-dictionary/)

## Approaches:

1. [Brute Force Approach](#1-brute-force-approach)
2. [Trie-based Approach](#2-trie-based-approach)

### 1. Brute Force Approach

#### Intuition:
The brute force approach involves iterating through each word and checking if all its prefixes exist in the given list. We can achieve this by using a HashSet to store all words and checking if each prefix of a word exists in the set. If so, keep track of the longest valid word encountered so far.

#### Detailed Steps:
- Insert all words into a HashSet for fast look-up.
- Iterate over each word. For each word, check if all of its prefixes exist using the HashSet.
- If all prefixes of a word exist, compare it with the current longest word and update if necessary.
- In cases where two valid words have the same length, choose the lexicographical smaller one.

```java
import java.util.HashSet;
import java.util.Set;

public class Solution {
    public String longestWord(String[] words) {
        // HashSet to keep all the words for quick lookup
        Set<String> wordSet = new HashSet<>();
        for (String word : words) {
            wordSet.add(word);
        }

        String result = "";
        // Iterate over each word to check if it can be built
        for (String word : words) {
            // Check if all prefixes of the word are present in the set
            boolean canBuild = true;
            for (int k = 1; k < word.length(); k++) {
                if (!wordSet.contains(word.substring(0, k))) {
                    canBuild = false;
                    break;
                }
            }
            // Condition to check if it's the longest or lexicographically smallest
            if (canBuild && (word.length() > result.length() || 
                             (word.length() == result.length() && word.compareTo(result) < 0))) {
                result = word;
            }
        }

        return result;
    }
}
```

#### Time Complexity:
- O(N * M), where N is the number of words and M is the maximum length of a word (as we need to check each prefix).

#### Space Complexity:
- O(N * M), to store all the words in the HashSet.

### 2. Trie-based Approach

#### Intuition:
A Trie is an efficient data structure to store and retrieve words, especially useful in prefix queries. By inserting all words into a Trie, we can efficiently check if a word can be built by other words. We traverse the Trie to find the deepest node that represents a complete buildable word, which will be our answer.

#### Detailed Steps:
- Insert all words into the Trie and mark the end of each complete word.
- Perform a depth-first search (DFS) on the Trie, keeping track of the longest word that can be constructed step-by-step.
- Continue traversal only if the current path represents a complete word until that node.

```java
import java.util.*;

public class Solution {
    static class TrieNode {
        TrieNode[] children = new TrieNode[26];
        boolean isEndOfWord = false;
        char character;
    }

    private TrieNode root;

    public Solution() {
        root = new TrieNode();
    }

    public String longestWord(String[] words) {
        // Insert all words into the Trie
        for (String word : words) {
            insert(word);
        }
        // Use a String to keep track of the longest valid (buildable) word
        return dfs(root, "");
    }

    private void insert(String word) {
        TrieNode current = root;
        for (char c : word.toCharArray()) {
            int index = c - 'a';
            if (current.children[index] == null) {
                current.children[index] = new TrieNode();
                current.children[index].character = c;
            }
            current = current.children[index];
        }
        current.isEndOfWord = true;
    }

    private String dfs(TrieNode node, String path) {
        String result = path;
        for (TrieNode child : node.children) {
            if (child != null && child.isEndOfWord) {
                String newWord = dfs(child, path + child.character);
                if (newWord.length() > result.length() || 
                    (newWord.length() == result.length() && newWord.compareTo(result) < 0)) {
                    result = newWord;
                }
            }
        }
        return result;
    }
}
```

#### Time Complexity:
- O(N * M), where N is the number of words and M is the maximum length of the words. Each insertion and search operation in the Trie is proportional to the length of the word.

#### Space Complexity:
- O(26 * N * M), for storing N words each having a maximum length M in the Trie. The 26 factor is for the 26 letters of the English alphabet.

