# 1268. [Search Suggestions System](https://leetcode.com/problems/search-suggestions-system/)

## Approach: Trie with Backtracking for Suggestions

### Solution
c#
// Time Complexity:
//   - Build Trie: O(n * m), where n is the number of products and m is the average length of a product
//   - Search Suggestions: O(m^2 + m * p), where m is the length of the search word and p is the average number of suggestions per prefix
// Space Complexity: O(n * m), where n is the number of products and m is the average length of a product

```csharp
using System;
using System.Collections.Generic;

public class SearchSuggestionsSystem
{
    private TrieNode root;

    public SearchSuggestionsSystem()
    {
        root = new TrieNode();
    }

    // Builds the Trie from a list of products
    public void BuildTrie(string[] products)
    {
        foreach (var product in products)
        {
            Insert(product);
        }
    }

    // Inserts a product into the Trie
    private void Insert(string product)
    {
        TrieNode node = root;
        foreach (var c in product)
        {
            if (!node.ContainsKey(c))
            {
                node.Put(c, new TrieNode());
            }
            node = node.Get(c);
            node.AddSuggestion(product); // Add the product as a suggestion for the prefix
        }
    }

    // Returns a list of suggested products for each prefix of the search word
    public List<List<string>> SuggestedProducts(string[] products, string searchWord)
    {
        List<List<string>> result = new List<List<string>>();
        BuildTrie(products);

        TrieNode node = root;
        foreach (var c in searchWord)
        {
            if (node != null)
            {
                node = node.Get(c); // Move to the child node
            }
            result.Add(node == null ? new List<string>() : node.GetSuggestions());
        }

        return result;
    }

    // Trie Node Class
    private class TrieNode
    {
        private readonly TrieNode[] links;
        private const int R = 26; // Number of possible children ('a' to 'z')
        private readonly List<string> suggestions;

        public TrieNode()
        {
            links = new TrieNode[R];
            suggestions = new List<string>();
        }

        public bool ContainsKey(char c)
        {
            return links[c - 'a'] != null;
        }

        public TrieNode Get(char c)
        {
            return links[c - 'a'];
        }

        public void Put(char c, TrieNode node)
        {
            links[c - 'a'] = node;
        }

        // Adds a product to the list of suggestions
        public void AddSuggestion(string product)
        {
            if (suggestions.Count < 3)
            { // Keep only the top 3 lexicographical suggestions
                suggestions.Add(product);
                suggestions.Sort();
                if (suggestions.Count > 3)
                {
                    suggestions.RemoveAt(3); // Remove extra suggestions if necessary
                }
            }
        }

        public List<string> GetSuggestions()
        {
            return suggestions;
        }
    }
}
```

