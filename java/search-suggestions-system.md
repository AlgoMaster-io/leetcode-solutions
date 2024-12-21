## [Leetcode 1268: Search Suggestions System](https://leetcode.com/problems/search-suggestions-system/)

### Approaches

- [Approach 1: Brute Force with Sorting](#approach-1-brute-force-with-sorting)
- [Approach 2: Prefix Trie with DFS](#approach-2-prefix-trie-with-dfs)
- [Approach 3: Two Pointers with Binary Search](#approach-3-two-pointers-with-binary-search)

---

### Approach 1: Brute Force with Sorting

**Intuition:**

The simplest solution to this problem is to sort the list of products first. For every typed prefix, we can iterate through all the products and check if a product starts with this prefix. If it does, we add it to our result list for that prefix until we have added 3 suggestions or exhausted the list.

**Steps:**

1. Sort the `products` array. 
2. For each prefix formed by typing the search word character by character:
   - Initialize an empty list to store suggestions.
   - Iterate through the sorted `products` list and check if the product starts with the current prefix.
   - Add the product to the suggestions list and stop if we gather 3 suggestions.
3. Append the list of suggestions for each prefix to the result.

```java
public List<List<String>> suggestedProducts(String[] products, String searchWord) {
    // Step 1: Sort the products array to ensure alphabetical order
    Arrays.sort(products);
    List<List<String>> result = new ArrayList<>();

    // Step 2: Loop through each character in searchWord to form prefixes
    for (int i = 0; i < searchWord.length(); i++) {
        String prefix = searchWord.substring(0, i + 1); // Current prefix
        List<String> suggestions = new ArrayList<>();

        // Step 3: Iterate through sorted products to find matching products
        for (String product : products) {
            if (product.startsWith(prefix)) {
                suggestions.add(product);
                if (suggestions.size() == 3) break; // Stop if we have 3 suggestions
            }
        }
        result.add(suggestions);
    }
    return result;
}
```

**Complexity Analysis:**

- Time Complexity: O(m * n + n log n), where `n` is the number of products and `m` is the length of the searchWord. Sorting costs O(n log n) and the nested loop costs O(m * n).
- Space Complexity: O(n), for storing the sorted list and results.

---

### Approach 2: Prefix Trie with DFS

**Intuition:**

We can use a Trie to represent all product names efficiently. For each character typed, we traverse the Trie to find the node representing that prefix and perform a DFS to gather the top 3 lexicographical suggestions.

**Steps:**

1. Build a Trie with all product names.
2. For each character typed:
   - Traverse the Trie to find the node representing the current prefix.
   - Perform a DFS from this node to collect up to 3 product suggestions.

```java
class TrieNode {
    TrieNode[] children = new TrieNode[26];
    List<String> suggestions = new ArrayList<>();
}

class Trie {
    TrieNode root = new TrieNode();
    
    public void insert(String product) {
        TrieNode node = root;
        for (char c : product.toCharArray()) {
            int index = c - 'a';
            if (node.children[index] == null) {
                node.children[index] = new TrieNode();
            }
            node = node.children[index];
            if (node.suggestions.size() < 3) {
                node.suggestions.add(product);
            }
        }
    }
    
    public List<String> search(String prefix) {
        TrieNode node = root;
        for (char c : prefix.toCharArray()) {
            int index = c - 'a';
            if (node.children[index] == null) {
                return new ArrayList<>();
            }
            node = node.children[index];
        }
        return node.suggestions;
    }
}

public List<List<String>> suggestedProducts(String[] products, String searchWord) {
    Trie trie = new Trie();
    Arrays.sort(products);
    
    for (String product : products) {
        trie.insert(product);
    }
    
    List<List<String>> result = new ArrayList<>();
    for (int i = 0; i < searchWord.length(); i++) {
        String prefix = searchWord.substring(0, i + 1);
        result.add(trie.search(prefix));
    }
    
    return result;
}
```

**Complexity Analysis:**

- Time Complexity: O(m + n log n + l * m), where `m` is the total number of characters in all products (for Trie insertion), `n log n` for sorting, and `l * m` is the sum of lengths of all prefixes multiplied by max suggestions (constant time for each prefix)
- Space Complexity: O(m), where `m` is the total number of characters stored in the Trie.

---

### Approach 3: Two Pointers with Binary Search

**Intuition:**

This approach utilizes binary search to find the starting point of the valid suggestions in the sorted products array. The addition of two pointers helps in efficiently reducing the search space as we progress through each character in the search word.

**Steps:**

1. Sort the `products` array.
2. Use binary search to efficiently find the starting index of products matching the current prefix.
3. Use two pointers to narrow down the range of possible suggestions for each new character typed.

```java
public List<List<String>> suggestedProducts(String[] products, String searchWord) {
    Arrays.sort(products);
    List<List<String>> result = new ArrayList<>();
    
    int left = 0;
    int right = products.length - 1;
    
    for (int i = 0; i < searchWord.length(); i++) {
        List<String> suggestions = new ArrayList<>();
        char c = searchWord.charAt(i);
        
        // Find the valid range using two pointers
        while (left <= right && (products[left].length() <= i || products[left].charAt(i) != c)) {
            left++;
        }
        while (left <= right && (products[right].length() <= i || products[right].charAt(i) != c)) {
            right--;
        }
        
        // Collect up to 3 suggestions
        for (int j = left; j < Math.min(left + 3, right + 1); j++) {
            suggestions.add(products[j]);
        }
        
        result.add(suggestions);
    }
    
    return result;
}
```

**Complexity Analysis:**

- Time Complexity: O(m + n log n), where `m` is the length of the searchWord, and `n` is the number of products. Sorting takes O(n log n), and the search using two pointers costs O(m) for narrowing the range for each prefix.
- Space Complexity: O(1) (apart from the space needed for output), since we are not using extra space for structures other than the list for output.

---

Each method provides a different perspective and efficiency on handling the current problem, with the Trie approach being particularly suited for scenarios with frequent prefix queries, and the two-pointers approach offering a balance between simplicity and performance.

