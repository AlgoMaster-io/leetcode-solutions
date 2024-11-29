# 720. [Longest Word in Dictionary](https://leetcode.com/problems/longest-word-in-dictionary/)

## Approach 1: Sort and HashSet

### Solution
```csharp
// Time Complexity: O(n * k), where n is the number of words and k is the average length of a word
// Space Complexity: O(n * k)
using System;
using System.Collections.Generic;

public class Solution {
    public string LongestWord(string[] words) {
        Array.Sort(words); // Sort words lexicographically
        HashSet<string> built = new HashSet<string>();
        string result = "";

        foreach (string word in words) {
            // Check if the prefix (word without the last character) is in the set
            if (word.Length == 1 || built.Contains(word.Substring(0, word.Length - 1))) {
                built.Add(word); // Add the word to the set
                // Update result if the word is longer or lexicographically smaller
                if (word.Length > result.Length || 
                    (word.Length == result.Length && string.Compare(word, result) < 0)) {
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
```csharp
// Time Complexity: O(n * k), where n is the number of words and k is the average length of a word
// Space Complexity: O(n * k)
using System;
using System.Collections.Generic;

class TrieNode {
    public Dictionary<char, TrieNode> Children = new Dictionary<char, TrieNode>();
    public bool IsEndOfWord = false;
    public string Word = "";
}

public class Solution {
    public string LongestWord(string[] words) {
        TrieNode root = new TrieNode();

        // Build the Trie
        foreach (string word in words) {
            TrieNode current = root;
            foreach (char c in word.ToCharArray()) {
                if (!current.Children.ContainsKey(c)) {
                    current.Children[c] = new TrieNode();
                }
                current = current.Children[c];
            }
            current.IsEndOfWord = true;
            current.Word = word;
        }

        // Perform DFS to find the longest word
        return Dfs(root, "");
    }

    private string Dfs(TrieNode node, string result) {
        if (node == null || (!node.IsEndOfWord && node.Word != null)) {
            return result;
        }

        // Check for the longest word in the current branch
        if (node.Word.Length > result.Length || 
            (node.Word.Length == result.Length && string.Compare(node.Word, result) < 0)) {
            result = node.Word;
        }

        for (char c = 'a'; c <= 'z'; c++) {
            if (node.Children.ContainsKey(c)) {
                result = Dfs(node.Children[c], result);
            }
        }

        return result;
    }
}
```

