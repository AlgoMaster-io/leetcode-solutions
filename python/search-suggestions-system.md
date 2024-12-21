[Leetcode Problem 1268: Search Suggestions System](https://leetcode.com/problems/search-suggestions-system/)

## Approaches:
- [1. Naive Solution: Sort and Filter](#1-naive-solution-sort-and-filter)
- [2. Trie Data Structure](#2-trie-data-structure)

### 1. Naive Solution: Sort and Filter

#### Intuition:
The simplest approach here is to utilize Python's built-in sorting and filtering capabilities. The idea is to iterate over each prefix of the search word and seek potential matches in the sorted list of products. By sorting the product list initially, we can easily find suggestions that are lexicographically ordered.

#### Steps:
1. Sort the product list to ensure suggestions are in lexicographic order.
2. For each prefix incrementally built from the search word, collect all products that start with that prefix.
3. For each prefix, provide the first three matching products.

```python
def suggestedProducts(products, searchWord):
    # Step 1: Sort the products to get them in lexicographical order
    products.sort()
    # Initialize a result list to store the suggestions for each prefix
    result = []
    # Get the length of the search word
    n = len(searchWord)
    # Iterate through each prefix using slicing
    for i in range(1, n + 1):
        prefix = searchWord[:i]
        # Use list comprehension to filter products matching the current prefix
        suggestions = [product for product in products if product.startswith(prefix)]
        # Append the first three suggestions or less if less than three are available
        result.append(suggestions[:3])
    return result
```

- **Time Complexity**: O(m log m + n * m) where m is the number of products and n is the length of the search word. Sorting takes O(m log m), and filtering takes O(n * m).
- **Space Complexity**: O(1), not considering the output space.

### 2. Trie Data Structure

#### Intuition:
A more efficient approach leverages a Trie (prefix tree). A Trie efficiently supports prefix searching operations. By adding products to a Trie, we can incrementally search the tree based on each character of the search word, retrieving up to three suggestions for each prefix.

#### Steps:
1. Create a Trie and insert all the products.
2. Traverse the Trie with each character of the search word.
3. At each level, gather the top three lexicographic suggestions starting from the current prefix.

```python
class TrieNode:
    def __init__(self):
        self.children = {}
        self.suggestions = []

class Trie:
    def __init__(self):
        self.root = TrieNode()

    def insert(self, word):
        node = self.root
        for char in word:
            if char not in node.children:
                node.children[char] = TrieNode()
            node = node.children[char]
            # Maintain a sorted list of up to three suggestions
            if len(node.suggestions) < 3:
                node.suggestions.append(word)

    def getSuggestions(self, prefix):
        node = self.root
        for char in prefix:
            if char in node.children:
                node = node.children[char]
            else:
                return []
        return node.suggestions

def suggestedProducts(products, searchWord):
    trie = Trie()
    # Step 1: First sort lexicographically to maintain sorted suggestions in Trie
    products.sort()
    # Step 2: Insert each product into the Trie
    for product in products:
        trie.insert(product)
    # Step 3: Build the result list by finding suggestions for each prefix of the search word
    result, prefix = [], ""
    for char in searchWord:
        prefix += char
        result.append(trie.getSuggestions(prefix))
    return result
```

- **Time Complexity**: O(m * l + n * l) where m is the number of products, l is the average length of a product, and n is the length of the search word. Trie insertion and search are performed in O(l), where l is the string length.
- **Space Complexity**: O(m * l), considering the storage of the Trie nodes.

These approaches showcase both straightforward and more efficient strategies to solve the problem, demonstrating the power of data structures like Tries for string manipulations.

