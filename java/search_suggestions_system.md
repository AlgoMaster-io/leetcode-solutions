# 1268. [Search Suggestions System](https://leetcode.com/problems/search-suggestions-system/)

## Approach: Trie with Backtracking for Suggestions

### Solution
```java
// Time Complexity:
//   - Build Trie: O(n * m), where n is the number of products and m is the average length of a product
//   - Search Suggestions: O(m^2 + m * p), where m is the length of the search word and p is the average number of suggestions per prefix
// Space Complexity: O(n * m), where n is the number of products and m is the average length of a product

import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

class SearchSuggestionsSystem {

    private TrieNode root;

    public SearchSuggestionsSystem() {
        root = new TrieNode();
    }

    // Builds the Trie from a list of products
    public void buildTrie(String[] products) {
        for (String product : products) {
            insert(product);
        }
    }

    // Inserts a product into the Trie
    private void insert(String product) {
        TrieNode node = root;
        for (char c : product.toCharArray()) {
            if (!node.containsKey(c)) {
                node.put(c, new TrieNode());
            }
            node = node.get(c);
            node.addSuggestion(product); // Add the product as a suggestion for the prefix
        }
    }

    // Returns a list of suggested products for each prefix of the search word
    public List<List<String>> suggestedProducts(String[] products, String searchWord) {
        List<List<String>> result = new ArrayList<>();
        buildTrie(products);

        TrieNode node = root;
        for (char c : searchWord.toCharArray()) {
            if (node != null) {
                node = node.get(c); // Move to the child node
            }
            result.add(node == null ? new ArrayList<>() : node.getSuggestions());
        }

        return result;
    }

    // Trie Node Class
    class TrieNode {
        private TrieNode[] links;
        private final int R = 26; // Number of possible children ('a' to 'z')
        private List<String> suggestions;

        public TrieNode() {
            links = new TrieNode[R];
            suggestions = new ArrayList<>();
        }

        public boolean containsKey(char c) {
            return links[c - 'a'] != null;
        }

        public TrieNode get(char c) {
            return links[c - 'a'];
        }

        public void put(char c, TrieNode node) {
            links[c - 'a'] = node;
        }

        // Adds a product to the list of suggestions
        public void addSuggestion(String product) {
            if (suggestions.size() < 3) { // Keep only the top 3 lexicographical suggestions
                suggestions.add(product);
                Collections.sort(suggestions);
                if (suggestions.size() > 3) {
                    suggestions.remove(3); // Remove extra suggestions if necessary
                }
            }
        }

        public List<String> getSuggestions() {
            return suggestions;
        }
    }
}
```