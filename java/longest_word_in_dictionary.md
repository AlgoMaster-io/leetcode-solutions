# 720. [Longest Word in Dictionary](https://leetcode.com/problems/longest-word-in-dictionary/)

## Approach 1: Sort and HashSet

### Solution
```java
// Time Complexity: O(n * k), where n is the number of words and k is the average length of a word
// Space Complexity: O(n * k)
import java.util.Arrays;
import java.util.HashSet;

public class Solution {
    public String longestWord(String[] words) {
        Arrays.sort(words); // Sort words lexicographically
        HashSet<String> built = new HashSet<>();
        String result = "";

        for (String word : words) {
            // Check if the prefix (word without the last character) is in the set
            if (word.length() == 1 || built.contains(word.substring(0, word.length() - 1))) {
                built.add(word); // Add the word to the set
                // Update result if the word is longer or lexicographically smaller
                if (word.length() > result.length() || 
                    (word.length() == result.length() && word.compareTo(result) < 0)) {
                    result = word;
                }
            }
        }

        return result;
    }
}
```

## Approach 2: Trie-Based Solution

### Solution
```java
// Time Complexity: O(n * k), where n is the number of words and k is the average length of a word
// Space Complexity: O(n * k)
import java.util.HashMap;

class TrieNode {
    HashMap<Character, TrieNode> children = new HashMap<>();
    boolean isEndOfWord = false;
    String word = "";
}

public class Solution {
    public String longestWord(String[] words) {
        TrieNode root = new TrieNode();

        // Build the Trie
        for (String word : words) {
            TrieNode current = root;
            for (char c : word.toCharArray()) {
                current.children.putIfAbsent(c, new TrieNode());
                current = current.children.get(c);
            }
            current.isEndOfWord = true;
            current.word = word;
        }

        // Perform DFS to find the longest word
        return dfs(root, "");
    }

    private String dfs(TrieNode node, String result) {
        if (node == null || (!node.isEndOfWord && node != null)) {
            return result;
        }

        // Check for the longest word in the current branch
        if (node.word.length() > result.length() || 
            (node.word.length() == result.length() && node.word.compareTo(result) < 0)) {
            result = node.word;
        }

        for (char c = 'a'; c <= 'z'; c++) {
            result = dfs(node.children.get(c), result);
        }

        return result;
    }
}
```