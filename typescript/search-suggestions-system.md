# [Leetcode 1268: Search Suggestions System](https://leetcode.com/problems/search-suggestions-system/)

## Approaches
- [Brute Force Search](#brute-force-search)
- [Sorting and Binary Search](#sorting-and-binary-search)
- [Trie-based Solution](#trie-based-solution)

---

## Brute Force Search

### Approach
The brute force approach involves iterating over the `products` list and filtering out those strings that match the current prefix created by the `searchWord`. For each prefix, extract the relevant products and then sort them to find up to three lexicographically smallest products.

### Steps:
1. Initialize an empty list `results` which will ultimately hold the lists of suggestions for each prefix.
2. For each prefix of `searchWord` (starting from the first character up to the entire word):
   - Filter the `products` to only include those starting with the current prefix.
   - Sort the filtered list lexicographically.
   - Take the first three elements of this sorted list as suggestions.
   - Append these suggestions to `results`.
3. Return `results`.

### Code

```typescript
function suggestedProducts(products: string[], searchWord: string): string[][] {
    const results: string[][] = [];
    
    for (let i = 1; i <= searchWord.length; i++) {
        // Current prefix
        const prefix = searchWord.substring(0, i);
        
        // Filter products that start with this prefix
        const filtered = products.filter(product => product.startsWith(prefix));
        
        // Sort and take up to the first three products
        filtered.sort((a, b) => a.localeCompare(b));
        
        // Add suggestions to the results
        results.push(filtered.slice(0, 3));
    }
    
    return results;
}
```

### Complexity
- **Time Complexity**: O(n * m + m log m), where n is the length of `searchWord`, and m is the length of `products`. The sorting step impacts the complexity significantly.
- **Space Complexity**: O(n * k), where k is the average size of the suggestions.

---

## Sorting and Binary Search

### Approach
Firstly, sort the list of products. For each prefix of `searchWord`, use binary search to find the starting position of products that match the current prefix. This efficiently narrows down potential matches.

### Steps:
1. Sort the `products` list.
2. Iterate over each character position in `searchWord` and form a prefix.
3. Use binary search (via built-in functions or a library) to locate the first item in `products` list that matches the prefix.
4. Collect up to three products from this position and append to `results`.
5. Return `results`.

### Code

```typescript
function suggestedProducts(products: string[], searchWord: string): string[][] {
    products.sort((a, b) => a.localeCompare(b));
    const results: string[][] = [];
    
    for (let i = 1; i <= searchWord.length; i++) {
        const prefix = searchWord.substring(0, i);
        let start = lowerBound(products, prefix);
        
        const suggestions: string[] = [];
        for (let j = 0; j < 3 && start + j < products.length; j++) {
            if (products[start + j].startsWith(prefix)) {
                suggestions.push(products[start + j]);
            }
        }
        
        results.push(suggestions);
    }
    
    return results;
}

function lowerBound(products: string[], target: string): number {
    let left = 0;
    let right = products.length;
    
    while (left < right) {
        const mid = Math.floor((left + right) / 2);
        if (products[mid].localeCompare(target) < 0) {
            left = mid + 1;
        } else {
            right = mid;
        }
    }
    
    return left;
}
```

### Complexity
- **Time Complexity**: O(m log m + n log m + nk), where n is the length of `searchWord`, m is the number of products, and k the max size of the suggestion lists.
- **Space Complexity**: O(n * k).

---

## Trie-based Solution

### Approach
Using a trie (prefix tree) can efficiently handle the search operations for each prefix as you only explore relevant parts of the data structure.

### Steps:
1. Construct a trie from the `products`.
2. For each prefix of `searchWord`, search down the trie to find potential matches.
3. For each prefix, collect up to three suggestions from the current trie node.
4. Append the collected suggestions to `results`.

### Code

```typescript
class TrieNode {
    suggestions: string[];
    children: Map<string, TrieNode>;
    constructor() {
        this.suggestions = [];
        this.children = new Map();
    }
}

function suggestedProducts(products: string[], searchWord: string): string[][] {
    const root = new TrieNode();
    
    // Build the trie
    for (const product of products) {
        let node = root;
        for (const char of product) {
            if (!node.children.has(char)) {
                node.children.set(char, new TrieNode());
            }
            node = node.children.get(char)!;
            if (node.suggestions.length < 3) {
                node.suggestions.push(product);
            }
        }
    }
    
    // Search for each prefix of searchWord
    const results: string[][] = [];
    let node = root;
    for (const char of searchWord) {
        if (node) {
            node = node.children.get(char) ?? null; 
            results.push(node ? node.suggestions : []);
        } else {
            results.push([]);
        }
    }
    
    return results;
}
```

### Complexity
- **Time Complexity**: O(m * l + n * l), where m is the length of `products`, l the average length of the product names, and n the length of `searchWord`.
- **Space Complexity**: O(26 * m * l), due to the storage of trie nodes and suggestions.

With these approaches, each provides a unique way of solving the search suggestion system challenge. The trie-based solution is often the most efficient when handling a large number of product prefixes.

