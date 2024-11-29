# 1268. [Search Suggestions System](https://leetcode.com/problems/search-suggestions-system/)

## Approach: Trie with Backtracking for Suggestions

### Solution
```python
# Time Complexity:
#   - Build Trie: O(n * m), where n is the number of products and m is the average length of a product
#   - Search Suggestions: O(m^2 + m * p), where m is the length of the search word and p is the average number of suggestions per prefix
# Space Complexity: O(n * m), where n is the number of products and m is the average length of a product

from collections import defaultdict

class TrieNode:
    def __init__(self):
        self.links = defaultdict(TrieNode)
        self.suggestions = []

    def add_suggestion(self, product):
        if len(self.suggestions) < 3:  # Keep only the top 3 lexicographical suggestions
            self.suggestions.append(product)
            self.suggestions.sort()
            if len(self.suggestions) > 3:
                self.suggestions.pop()  # Remove extra suggestions if necessary

    def get_suggestions(self):
        return self.suggestions

class SearchSuggestionsSystem:

    def __init__(self):
        self.root = TrieNode()

    # Builds the Trie from a list of products
    def build_trie(self, products):
        for product in products:
            self.insert(product)

    # Inserts a product into the Trie
    def insert(self, product):
        node = self.root
        for c in product:
            node = node.links[c]
            node.add_suggestion(product)  # Add the product as a suggestion for the prefix

    # Returns a list of suggested products for each prefix of the search word
    def suggested_products(self, products, search_word):
        result = []
        self.build_trie(products)

        node = self.root
        for c in search_word:
            if node:
                node = node.links.get(c)  # Move to the child node
            result.append([] if node is None else node.get_suggestions())

        return result
```

