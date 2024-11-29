# 1268. [Search Suggestions System](https://leetcode.com/problems/search-suggestions-system/)

## Approach: Trie with Backtracking for Suggestions

### Solution
javascript
```javascript
// Time Complexity:
//   - Build Trie: O(n * m), where n is the number of products and m is the average length of a product
//   - Search Suggestions: O(m^2 + m * p), where m is the length of the search word and p is the average number of suggestions per prefix
// Space Complexity: O(n * m), where n is the number of products and m is the average length of a product

class SearchSuggestionsSystem {
    constructor() {
        this.root = new TrieNode();
    }

    // Builds the Trie from a list of products
    buildTrie(products) {
        for (let product of products) {
            this.insert(product);
        }
    }

    // Inserts a product into the Trie
    insert(product) {
        let node = this.root;
        for (let c of product) {
            if (!node.containsKey(c)) {
                node.put(c, new TrieNode());
            }
            node = node.get(c);
            node.addSuggestion(product); // Add the product as a suggestion for the prefix
        }
    }

    // Returns a list of suggested products for each prefix of the search word
    suggestedProducts(products, searchWord) {
        let result = [];
        this.buildTrie(products);

        let node = this.root;
        for (let c of searchWord) {
            if (node !== null) {
                node = node.get(c); // Move to the child node
            }
            result.push(node === null ? [] : node.getSuggestions());
        }

        return result;
    }
}

class TrieNode {
    constructor() {
        this.links = new Array(26);
        this.suggestions = [];
    }

    containsKey(c) {
        return this.links[c.charCodeAt(0) - 'a'.charCodeAt(0)] !== undefined;
    }

    get(c) {
        return this.links[c.charCodeAt(0) - 'a'.charCodeAt(0)];
    }

    put(c, node) {
        this.links[c.charCodeAt(0) - 'a'.charCodeAt(0)] = node;
    }

    // Adds a product to the list of suggestions
    addSuggestion(product) {
        if (this.suggestions.length < 3) { // Keep only the top 3 lexicographical suggestions
            this.suggestions.push(product);
            this.suggestions.sort(); // Sort the list
            if (this.suggestions.length > 3) {
                this.suggestions.pop(); // Remove extra suggestions if necessary
            }
        }
    }

    getSuggestions() {
        return this.suggestions;
    }
}
```


