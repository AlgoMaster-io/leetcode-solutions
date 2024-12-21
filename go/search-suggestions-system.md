[Leetcode 1268: Search Suggestions System](https://leetcode.com/problems/search-suggestions-system/)

## Approaches
1. [Brute Force Approach](#approach-1-brute-force-approach)
2. [Sorting and Binary Search Approach](#approach-2-sorting-and-binary-search-approach)
3. [Trie-based Approach](#approach-3-trie-based-approach)

---

### Approach 1: Brute Force Approach

In this naive approach, for each prefix of the search word, we directly filter the products list to get matching products. We then sort the results and take the first three.

#### Intuition
- Traverse each prefix of the search word.
- Check each product to see if it starts with the current prefix.
- Sort matched products lexicographically.
- Store up to the first three suggestions.

#### Code

```go
func suggestedProducts(products []string, searchWord string) [][]string {
    result := [][]string{}
    
    for i := 1; i <= len(searchWord); i++ {
        prefix := searchWord[:i]
        matchingProducts := []string{}
        
        for _, product := range products {
            if len(product) >= i && product[:i] == prefix {
                matchingProducts = append(matchingProducts, product)
            }
        }
        
        // Sort the matchingProducts array lexicographically
        sort.Strings(matchingProducts)
        
        // Add the first three (or fewer) products to the result
        if len(matchingProducts) > 3 {
            matchingProducts = matchingProducts[:3]
        }
        
        result = append(result, matchingProducts)
    }
    
    return result
}
```

#### Time Complexity
- O(n * m * log m), where n is the length of the search word and m is the number of products.

#### Space Complexity
- O(m), to store matching products for each prefix.

---

### Approach 2: Sorting and Binary Search Approach

First, sort the products list. For each prefix of the search word, use binary search to find the starting position of matches. Gather up to three products starting from this position.

#### Intuition
- Sorting products allows binary search to quickly locate candidate products.
- Binary search helps find the first product match for the prefix efficiently.
- Add up to three products by checking subsequent positions.

#### Code

```go
func suggestedProducts(products []string, searchWord string) [][]string {
    sort.Strings(products)
    result := [][]string{}
    
    prefix := ""
    for i := 0; i < len(searchWord); i++ {
        prefix += string(searchWord[i])
        startIndex := sort.SearchStrings(products, prefix)
        
        // Collect the first three valid products starting from startIndex
        suggestions := []string{}
        for j := startIndex; j < len(products) && len(suggestions) < 3; j++ {
            if len(products[j]) >= len(prefix) && products[j][:len(prefix)] == prefix {
                suggestions = append(suggestions, products[j])
            } else {
                break
            }
        }
        
        result = append(result, suggestions)
    }
    
    return result
}
```

#### Time Complexity
- O(m * log m + n * 3), where m is the number of products and n is the length of the search word.

#### Space Complexity
- O(1), additional space other than the output.

---

### Approach 3: Trie-based Approach

Construct a Trie from the products list, which allows efficient retrieval of prefix-matching words.

#### Intuition
- Trie (prefix tree) naturally supports operations on prefixes.
- Efficiently stores products for prefix matching.
- Each Trie node maintains a sorted list of size at most 3.

#### Code

```go
type TrieNode struct {
    children map[rune]*TrieNode
    suggestions []string
}

type Trie struct {
    root *TrieNode
}

func NewTrie() *Trie {
    return &Trie{&TrieNode{children: make(map[rune]*TrieNode)}}
}

func (t *Trie) Insert(word string) {
    currNode := t.root
    for _, ch := range word {
        if _, found := currNode.children[ch]; !found {
            currNode.children[ch] = &TrieNode{children: make(map[rune]*TrieNode)}
        }
        currNode = currNode.children[ch]
        currNode.suggestions = append(currNode.suggestions, word)
        sort.Strings(currNode.suggestions)
        if len(currNode.suggestions) > 3 {
            currNode.suggestions = currNode.suggestions[:3]
        }
    }
}

func (t *Trie) Search(prefix string) []string {
    currNode := t.root
    for _, ch := range prefix {
        if nextNode, found := currNode.children[ch]; found {
            currNode = nextNode
        } else {
            return []string{}
        }
    }
    return currNode.suggestions
}

func suggestedProducts(products []string, searchWord string) [][]string {
    trie := NewTrie()
    for _, product := range products {
        trie.Insert(product)
    }
    
    result := [][]string{}
    for i := 1; i <= len(searchWord); i++ {
        prefix := searchWord[:i]
        suggestions := trie.Search(prefix)
        result = append(result, suggestions)
    }
    
    return result
}
```

#### Time Complexity
- O(L ∗ n + p), where L is the average length of a product, n is the number of products, and p is the total search prefix length.

#### Space Complexity
- O(L ∗ n), for storing products in the Trie.

This trie-based approach is efficient when there are many query prefixes, leveraging precomputed data structures.

