# [Leetcode 720: Longest Word in Dictionary](https://leetcode.com/problems/longest-word-in-dictionary/)

## Approaches:
- [Approach 1: Brute Force with Two Loops](#approach-1-brute-force-with-two-loops)
- [Approach 2: HashSet with Sorting](#approach-2-hashset-with-sorting)
- [Approach 3: Trie Data Structure](#approach-3-trie-data-structure)

---

## Approach 1: Brute Force with Two Loops

### Intuition:
The simplest way to solve this problem is by comparing each word with others to see if all prefixes of a word exist in the list. We use a nested loop to check if every prefix of a given word is present in the dictionary.

### Steps:
1. Iterate over each word.
2. For every word, check if all its prefixes are present by iterating again.
3. Keep track of the longest word which satisfies the condition.

### Code:
```csharp
public class Solution {
    public string LongestWord(string[] words) {
        Array.Sort(words); // Sort words to ensure lexicographical order
        HashSet<string> wordSet = new HashSet<string>(words);
        string longestWord = "";

        // Iterate over each word
        foreach (string word in words) {
            bool isValid = true;
            
            // Check if all prefixes of the word exist
            for (int k = 1; k < word.Length; k++) {
                if (!wordSet.Contains(word.Substring(0, k))) {
                    isValid = false;
                    break;
                }
            }
            
            // Check if word is valid and update longestWord
            if (isValid && word.Length > longestWord.Length) {
                longestWord = word;
            }
        }
        return longestWord;
    }
}
```

### Time Complexity:
- O(N * M²), where N is the number of words, and M is the average length of the words. Checking prefixes involves M steps.

### Space Complexity:
- O(N) for storing words in a hash set.

---

## Approach 2: HashSet with Sorting

### Intuition:
By using a HashSet, we can efficiently check the existence of prefixes. Additionally, sorting the words ensures we pick the lexicographically smallest word when lengths are equal.

### Steps:
1. Sort words to ensure lexicographical order. 
2. Use a HashSet to track the valid prefixes efficiently as we build words.
3. Track the longest valid word during iteration.

### Code:
```csharp
public class Solution {
    public string LongestWord(string[] words) {
        Array.Sort(words); // Sort lexicographically
        HashSet<string> validWords = new HashSet<string>();
        string longestWord = "";

        foreach (string word in words) {
            // For one letter words, add to valid set automatically
            if (word.Length == 1 || validWords.Contains(word.Substring(0, word.Length - 1))) {
                validWords.Add(word);
                if (word.Length > longestWord.Length) {
                    longestWord = word;
                }
            }
        }

        return longestWord;
    }
}
```

### Time Complexity:
- O(N * M), which is better than the previous approach due to skipping some iterations.

### Space Complexity:
- O(N) to store the valid prefixes.

---

## Approach 3: Trie Data Structure

### Intuition:
The Trie data structure is very efficient for problems involving prefixes since it naturally maps words by their common prefix. Construct a Trie and perform a traversal to find the longest valid word.

### Steps:
1. Construct a Trie from the given list of words.
2. Traverse nodes to find the longest valid word where every prefix is present in the Trie.

### Code:
```csharp
public class Solution {
    private class TrieNode {
        public Dictionary<char, TrieNode> children = new Dictionary<char, TrieNode>();
        public bool isEndOfWord = false;
        public string word = "";
    }

    private TrieNode root = new TrieNode();

    public string LongestWord(string[] words) {

        // Build the Trie structure
        foreach (string word in words) {
            AddWord(word);
        }

        return DFS(root);
    }

    private void AddWord(string word) {
        TrieNode current = root;
        foreach (char c in word) {
            if (!current.children.ContainsKey(c)) {
                current.children[c] = new TrieNode();
            }
            current = current.children[c];
        }
        current.isEndOfWord = true;
        current.word = word;
    }

    private string DFS(TrieNode node) {
        string result = node.word;
        foreach (var kvp in node.children) {
            TrieNode child = kvp.Value;
            if (child.isEndOfWord) {
                string childWord = DFS(child);
                if (childWord.Length > result.Length || 
                    (childWord.Length == result.Length && string.Compare(childWord, result) < 0)) {
                    result = childWord;
                }
            }
        }
        return result;
    }
}
```

### Time Complexity:
- O(N * M), where N is the number of words and M is the length of the longest word.

### Space Complexity:
- O(ΣM), where Σ represents the sum of lengths for all words in the dictionary to store them in the Trie.

---

Each approach progresses from a basic brute-force method to a more efficient Trie-based approach, optimizing both time and space usage. The Trie approach, in particular, offers a robust solution to efficiently handle larger inputs and longer words.

