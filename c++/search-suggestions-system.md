# [Leetcode 1268: Search Suggestions System](https://leetcode.com/problems/search-suggestions-system/)

## Approaches

- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Sorting and Binary Search](#approach-2-sorting-and-binary-search)
- [Approach 3: Trie](#approach-3-trie)

---

## Approach 1: Brute Force

### Intuition

The brute force approach attempts to create all possible prefixes from the search word and then, for each prefix, iterate through the list of products to find matches. For each prefix, the top 3 lexicographically smallest strings that start with that prefix are stored. This approach doesn’t use any data structure for quick lookup; hence, it is the least efficient.

### Steps

1. Iterate through the search word to create all possible prefixes.
2. For each prefix, iterate through the list of products.
3. Check if the current product matches the prefix.
4. Sort the matched products and take the top 3.

### Time and Space Complexity

- **Time Complexity:** O(n * m * log(m)), where `n` is the length of the search word, and `m` is the length of the products list.
- **Space Complexity:** O(m) to store matched products.

### C++ Code

```cpp
vector<vector<string>> suggestedProducts(vector<string>& products, string searchWord) {
    vector<vector<string>> result;
    
    // Generate each prefix of the searchWord
    for (int i = 1; i <= searchWord.size(); ++i) {
        string prefix = searchWord.substr(0, i);
        vector<string> matchedProducts;

        // Check each product if it starts with the current prefix
        for (const auto& product : products) {
            if (product.find(prefix) == 0) {
                matchedProducts.push_back(product);
            }
        }

        // Sort the matched products lexicographically
        sort(matchedProducts.begin(), matchedProducts.end());

        // Only take the top 3 lexicographically smallest
        if (matchedProducts.size() > 3) {
            matchedProducts.resize(3);
        }
        
        result.push_back(matchedProducts);
    }
    
    return result;
}
```

---

## Approach 2: Sorting and Binary Search

### Intuition

This approach improves on brute force by pre-sorting the products and using binary search to find the start position of matches. This allows us to avoid iterating through each product for every prefix, reducing time complexity significantly for large inputs.

### Steps

1. Sort the list of products.
2. For each prefix of the search word:
   - Use binary search to find the index where products start with the prefix.
   - Collect the next 3 products starting from that index.

### Time and Space Complexity

- **Time Complexity:** O(m * log(m) + n * log(m)), where `m` is the number of products, and `n` is the length of the search word.
- **Space Complexity:** O(1) additional space, not counting the output list.

### C++ Code

```cpp
vector<vector<string>> suggestedProducts(vector<string>& products, string searchWord) {
    vector<vector<string>> result;
    
    // Sort products lexicographically
    sort(products.begin(), products.end());

    string prefix;
    for (char ch : searchWord) {
        prefix += ch;
        
        // Find the insertion index using binary search
        auto it = lower_bound(products.begin(), products.end(), prefix);
        
        // Start collecting matching products starting from 'it'
        vector<string> matchedProducts;
        for (int i = 0; i < 3 && it + i < products.end(); ++i) {
            // Check if the current product matches the prefix
            if ((it + i)->substr(0, prefix.size()) == prefix) {
                matchedProducts.push_back(*(it + i));
            }
        }
        
        result.push_back(matchedProducts);
    }
    
    return result;
}
```

---

## Approach 3: Trie

### Intuition

Using a Trie is optimal for prefix searches. We build a Trie with all the products and for each prefix of the search word, we descend the Trie to gather suggestions. This approach provides fast lookups and helps to efficiently manage the suggestions.

### Steps

1. Build a Trie where each node contains a list of products that pass through that node, limited to 3 products.
2. For each prefix in the search word, traverse the Trie.
3. Collect products stored in the terminal node for that prefix.

### Time and Space Complexity

- **Time Complexity:** O(n + m * length_of_max_product), where `n` is the length of the search word.
- **Space Complexity:** O(m * length_of_largest_common_prefix), which essentially accounts for the Trie structure.

### C++ Code

```cpp
class TrieNode {
public:
    vector<string> suggestions;
    unordered_map<char, TrieNode*> children;
};

class Trie {
public:
    TrieNode* root;
    
    Trie() : root(new TrieNode()) {}
    
    void insert(const string& product) {
        TrieNode* node = root;
        for (char ch : product) {
            if (!node->children.count(ch)) {
                node->children[ch] = new TrieNode();
            }
            node = node->children[ch];
            // Keep the top 3 lexicographically smallest products
            if (node->suggestions.size() < 3) {
                node->suggestions.push_back(product);
            }
        }
    }
    
    vector<string> search(const string& prefix) {
        TrieNode* node = root;
        for (char ch : prefix) {
            if (!node->children.count(ch)) {
                return {}; // No matches
            }
            node = node->children[ch];
        }
        return node->suggestions;
    }
};

vector<vector<string>> suggestedProducts(vector<string>& products, string searchWord) {
    Trie trie;
    
    // Sort products before inserting into Trie for lexicographical order
    sort(products.begin(), products.end());

    // Insert each product into the Trie
    for (const string& product : products) {
        trie.insert(product);
    }

    vector<vector<string>> result;
    string prefix;
    for (char ch : searchWord) {
        prefix += ch;
        result.push_back(trie.search(prefix));
    }
    
    return result;
}
```

This completes the most efficient solution to the problem using a Trie, allowing fast prefix queries with minimal additional storage during suggestions lookup.

