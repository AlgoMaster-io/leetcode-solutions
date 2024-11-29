# 1268. [Search Suggestions System](https://leetcode.com/problems/search-suggestions-system/)

## Approach: Trie with Backtracking for Suggestions

### Solution
```go
// Time Complexity:
//   - Build Trie: O(n * m), where n is the number of products and m is the average length of a product
//   - Search Suggestions: O(m^2 + m * p), where m is the length of the search word and p is the average number of suggestions per prefix
// Space Complexity: O(n * m), where n is the number of products and m is the average length of a product

package main

import (
	"sort"
)

type SearchSuggestionsSystem struct {
	root *TrieNode
}

type TrieNode struct {
	links      [26]*TrieNode
	suggestions []string
}

func Constructor() SearchSuggestionsSystem {
	return SearchSuggestionsSystem{root: &TrieNode{}}
}

// Builds the Trie from a list of products
func (s *SearchSuggestionsSystem) buildTrie(products []string) {
	for _, product := range products {
		s.insert(product)
	}
}

// Inserts a product into the Trie
func (s *SearchSuggestionsSystem) insert(product string) {
	node := s.root
	for i := 0; i < len(product); i++ {
		c := product[i] - 'a'
		if node.links[c] == nil {
			node.links[c] = &TrieNode{}
		}
		node = node.links[c]
		node.addSuggestion(product) // Add the product as a suggestion for the prefix
	}
}

// Returns a list of suggested products for each prefix of the search word
func (s *SearchSuggestionsSystem) suggestedProducts(products []string, searchWord string) [][]string {
	var result [][]string
	s.buildTrie(products)

	node := s.root
	for i := 0; i < len(searchWord); i++ {
		c := searchWord[i] - 'a'
		if node != nil && node.links[c] != nil {
			node = node.links[c] // Move to the child node
		} else {
			node = nil
		}
		if node == nil {
			result = append(result, []string{})
		} else {
			result = append(result, node.getSuggestions())
		}
	}

	return result
}

// Adds a product to the list of suggestions
func (node *TrieNode) addSuggestion(product string) {
	if len(node.suggestions) < 3 {
		node.suggestions = append(node.suggestions, product)
		sort.Strings(node.suggestions)
		if len(node.suggestions) > 3 {
			node.suggestions = node.suggestions[:3] // Keep only the top 3 lexicographical suggestions
		}
	}
}

// Returns the list of suggestions
func (node *TrieNode) getSuggestions() []string {
	return node.suggestions
}
```

