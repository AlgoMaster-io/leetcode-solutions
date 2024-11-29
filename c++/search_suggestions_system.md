# 1268. [Search Suggestions System](https://leetcode.com/problems/search-suggestions-system/)

## Approach: Trie with Backtracking for Suggestions

### Solution
```cpp
// Time Complexity:
//   - Build Trie: O(n * m), where n is the number of products and m is the average length of a product
//   - Search Suggestions: O(m^2 + m * p), where m is the length of the search word and p is the average number of suggestions per prefix
// Space Complexity: O(n * m), where n is the number of products and m is the average length of a product

#include <vector>
#include <string>
#include <algorithm>

using namespace std;

class SearchSuggestionsSystem {

private:
    struct TrieNode {
        TrieNode* links[26];
        vector<string> suggestions;
        
        TrieNode() {
            fill(begin(links), end(links), nullptr);
        }

        bool containsKey(char c) {
            return links[c - 'a'] != nullptr;
        }

        TrieNode* get(char c) {
            return links[c - 'a'];
        }

        void put(char c, TrieNode* node) {
            links[c - 'a'] = node;
        }

        // Adds a product to the list of suggestions
        void addSuggestion(const string& product) {
            if (suggestions.size() < 3) { // Keep only the top 3 lexicographical suggestions
                suggestions.push_back(product);
                sort(suggestions.begin(), suggestions.end());
                if (suggestions.size() > 3) {
                    suggestions.pop_back(); // Remove extra suggestions if necessary
                }
            }
        }

        vector<string> getSuggestions() {
            return suggestions;
        }
    };

    TrieNode* root;

public:
    SearchSuggestionsSystem() {
        root = new TrieNode();
    }

    // Builds the Trie from a list of products
    void buildTrie(const vector<string>& products) {
        for (const string& product : products) {
            insert(product);
        }
    }

    // Inserts a product into the Trie
    void insert(const string& product) {
        TrieNode* node = root;
        for (char c : product) {
            if (!node->containsKey(c)) {
                node->put(c, new TrieNode());
            }
            node = node->get(c);
            node->addSuggestion(product); // Add the product as a suggestion for the prefix
        }
    }

    // Returns a list of suggested products for each prefix of the search word
    vector<vector<string>> suggestedProducts(const vector<string>& products, const string& searchWord) {
        vector<vector<string>> result;
        buildTrie(products);

        TrieNode* node = root;
        for (char c : searchWord) {
            if (node != nullptr) {
                node = node->get(c); // Move to the child node
            }
            result.push_back(node == nullptr ? vector<string>() : node->getSuggestions());
        }

        return result;
    }
};
```

