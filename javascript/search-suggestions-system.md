# [1268. Search Suggestions System](https://leetcode.com/problems/search-suggestions-system/)

## Solutions
1. [Brute Force Solution](#brute-force)
2. [Binary Search Solution](#binary-search)
3. [Trie Solution](#trie)

### Brute Force Solution

The brute force approach is straightforward: as the user types characters into the search bar, filter through the `products` list to find matching products and then sort them. We'll repeat this process for each substring of the search word, starting with the first character and adding one character at a time.

#### Intuition
- For each prefix of the search word, iterate through the list of products.
- Filter products that start with the current prefix.
- Sort the filtered products and select up to the first three matches.

```javascript
function suggestedProducts(products, searchWord) {
    // Sort the products array lexicographically
    products.sort();
    
    let result = [];
    let prefix = "";

    // Build the prefix incrementally
    for (let char of searchWord) {
        prefix += char;
        let suggestions = [];
        
        // Collect products matching the current prefix
        for (let product of products) {
            if (product.startsWith(prefix)) {
                suggestions.push(product);
                // We only need the first three matches
                if (suggestions.length === 3) break;
            }
        }
        
        // Add the suggestions for this prefix
        result.push(suggestions);
    }
    
    return result;
}
```

**Time Complexity**: O(n * m + m * log m)  
**Space Complexity**: O(m), where n is the number of products and m is the average length of a product string.

### Binary Search Solution

Instead of iterating through the entire list for each prefix, we can use binary search to efficiently find the starting position of matches in a sorted product list. Once the starting position is found, at most three suggestions are directly taken from the list.

#### Intuition
- With the list of products sorted, use binary search to find the position of the first product matching the current prefix.
- Collect up to three consecutive products starting from this position (if they match the prefix).

```javascript
function suggestedProducts(products, searchWord) {
    // Sort products lexicographically
    products.sort();

    const result = [];
    let prefix = "";

    for (let char of searchWord) {
        prefix += char;
        // Binary search to find the insert position of the current prefix
        let start = binarySearch(products, prefix);

        // Collect up to 3 suggestions starting from the found position
        let suggestions = [];
        for (let i = start; i < Math.min(start + 3, products.length); i++) {
            if (products[i].startsWith(prefix)) {
                suggestions.push(products[i]);
            }
        }

        result.push(suggestions);
    }

    return result;
}

function binarySearch(products, prefix) {
    let low = 0;
    let high = products.length;
    while (low < high) {
        let mid = Math.floor((low + high) / 2);
        if (products[mid] < prefix) {
            low = mid + 1;
        } else {
            high = mid;
        }
    }
    return low;
}
```

**Time Complexity**: O(m * log n + m)  
**Space Complexity**: O(1), where n is the number of products and m is the number of characters in searchWord.

### Trie Solution

Utilize a Trie (Prefix Tree), which is a specialized data structure to efficiently handle prefix-based operations. Each node represents a character, and a path in the Trie represents a word prefix.

#### Intuition
- Insert all products into a Trie.
- For each prefix of the search word, traverse the Trie to collect up to three lexicographically smallest words.

```javascript
class TrieNode {
    constructor() {
        this.children = {};
        this.suggestions = [];
    }
}

function insertProduct(root, product) {
    let node = root;
    for (const char of product) {
        if (!node.children[char]) {
            node.children[char] = new TrieNode();
        }
        node = node.children[char];
        
        // Add the product to suggestions if less than 3
        if (node.suggestions.length < 3) {
            node.suggestions.push(product);
        }
    }
}

function searchSuggestionsInTrie(root, searchWord) {
    let result = [];
    let node = root;
    
    for (const char of searchWord) {
        if (node) node = node.children[char];
        result.push(node ? node.suggestions : []);
    }
    
    return result;
}

function suggestedProducts(products, searchWord) {
    const root = new TrieNode();
    products.sort(); // Ensure lexicographical order
    
    // Insert each product into the Trie
    for (const product of products) {
        insertProduct(root, product);
    }

    // Fetch the search suggestions for each prefix
    return searchSuggestionsInTrie(root, searchWord);
}
```

**Time Complexity**: O(M * L), where M is the number of products and L is the length of the largest word.  
**Space Complexity**: O(n * L), where n is the number of nodes in the Trie and L is the average length of a product.

