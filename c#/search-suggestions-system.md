# Problem: [1268. Search Suggestions System](https://leetcode.com/problems/search-suggestions-system/)

## Approaches
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Trie Data Structure](#approach-2-trie-data-structure)

---

## Approach 1: Brute Force

### Intuition
The brute force approach involves sorting the list of products and then iteratively building the prefix from the search word. For each prefix, we scan through the entire list of products to find up to three suggestions that match the prefix.

### Code
```csharp
using System;
using System.Collections.Generic;

public class Solution {
    public IList<IList<string>> SuggestedProducts(string[] products, string searchWord) {
        // Sort the products to prepare for lexicographical ordering
        Array.Sort(products);
        
        var result = new List<IList<string>>();
        
        for (int i = 1; i <= searchWord.Length; i++) {
            // Extract the prefix from the search word
            string prefix = searchWord.Substring(0, i);
            var suggestions = new List<string>();
            
            foreach (var product in products) {
                // Check if the product starts with the current prefix
                if (product.StartsWith(prefix)) {
                    suggestions.Add(product);
                }
                
                // Stop once three suggestions have been added
                if (suggestions.Count == 3) {
                    break;
                }
            }
            
            // Add the suggestions for the current prefix to the result list
            result.Add(suggestions);
        }
        
        return result;
    }
}
```

### Time and Space Complexity
- **Time Complexity**: O(N log N + N * M), where N is the number of products and M is the length of the search word. Sorting takes O(N log N), and the worst-case might require checking each of N products for M prefixes.
- **Space Complexity**: O(N), for storing the sorted list of products and the resulting suggestions.

---

## Approach 2: Trie Data Structure

### Intuition
Using a Trie (prefix tree) is optimal for scenarios involving prefix searches. We first build a Trie from the list of products. For each prefix of the search word, we traverse the Trie to collect up to three lexicographically sorted products that start with that prefix.

### Code
```csharp
using System;
using System.Collections.Generic;

public class Solution {
    // TrieNode class definition
    class TrieNode {
        public Dictionary<char, TrieNode> Children = new Dictionary<char, TrieNode>();
        public SortedSet<string> Words = new SortedSet<string>();
    }

    // Inserts a product into the Trie
    private void Insert(TrieNode root, string product) {
        var node = root;
        foreach (var ch in product) {
            if (!node.Children.ContainsKey(ch)) {
                node.Children[ch] = new TrieNode();
            }
            node = node.Children[ch];
            if (node.Words.Count < 3) {
                node.Words.Add(product);
            }
        }
    }

    // Get suggestions by traversing the Trie
    private IList<string> GetSuggestions(TrieNode node) {
        var suggestions = new List<string>();
        foreach (var word in node.Words) {
            suggestions.Add(word);
            if (suggestions.Count == 3) break;
        }
        return suggestions;
    }

    public IList<IList<string>> SuggestedProducts(string[] products, string searchWord) {
        // Initializing the Trie root
        var root = new TrieNode();

        // Insert all products into the Trie
        foreach (var product in products) {
            Insert(root, product);
        }

        var result = new List<IList<string>>();
        var currentNode = root;

        foreach (var ch in searchWord) {
            if (currentNode != null && currentNode.Children.ContainsKey(ch)) {
                currentNode = currentNode.Children[ch];
            } else {
                currentNode = null;
            }

            // If currentNode is null, add an empty list (no matches found)
            result.Add(currentNode == null ? new List<string>() : GetSuggestions(currentNode));
        }

        return result;
    }
}
```

### Time and Space Complexity
- **Time Complexity**: O(N * L + M), where N is the number of products, L is the average length of a product (for Trie insertion), and M is the length of the search word (for searching).
- **Space Complexity**: O(T), where T is the total number of characters in all products, to store them in the Trie. The space used for result lists is at most O(M * 3).

This Trie-based approach optimizes the prefix search significantly by leveraging the prefix tree structure, reducing unnecessary traversals compared to the brute force method.

