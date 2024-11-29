# 1268. [Search Suggestions System](https://leetcode.com/problems/search-suggestions-system/)

## Approach: Trie with Backtracking for Suggestions

### Solution
typescript
```typescript
// Time Complexity:
//   - Build Trie: O(n * m), where n is the number of products and m is the average length of a product
//   - Search Suggestions: O(m^2 + m * p), where m is the length of the search word and p is the average number of suggestions per prefix
// Space Complexity: O(n * m), where n is the number of products and m is the average length of a product

class TrieNode {
  private links: Array<TrieNode | null> = new Array(26).fill(null);
  private suggestions: string[] = [];

  public containsKey(c: string): boolean {
    return this.links[c.charCodeAt(0) - 'a'.charCodeAt(0)] !== null;
  }

  public get(c: string): TrieNode | null {
    return this.links[c.charCodeAt(0) - 'a'.charCodeAt(0)];
  }

  public put(c: string, node: TrieNode): void {
    this.links[c.charCodeAt(0) - 'a'.charCodeAt(0)] = node;
  }

  public addSuggestion(product: string): void {
    if (this.suggestions.length < 3) {
      this.suggestions.push(product);
      this.suggestions.sort((a, b) => a.localeCompare(b));
      if (this.suggestions.length > 3) {
        this.suggestions.pop();
      }
    }
  }

  public getSuggestions(): string[] {
    return this.suggestions;
  }
}

class SearchSuggestionsSystem {
  private root: TrieNode;

  constructor() {
    this.root = new TrieNode();
  }

  // Builds the Trie from a list of products
  public buildTrie(products: string[]): void {
    for (const product of products) {
      this.insert(product);
    }
  }

  // Inserts a product into the Trie
  private insert(product: string): void {
    let node: TrieNode | null = this.root;
    for (const c of product) {
      if (!node.containsKey(c)) {
        node.put(c, new TrieNode());
      }
      node = node.get(c)!;
      node.addSuggestion(product); // Add the product as a suggestion for the prefix
    }
  }

  // Returns a list of suggested products for each prefix of the search word
  public suggestedProducts(products: string[], searchWord: string): string[][] {
    const result: string[][] = [];
    this.buildTrie(products);

    let node: TrieNode | null = this.root;
    for (const c of searchWord) {
      if (node !== null) {
        node = node.get(c); // Move to the child node
      }
      result.push(node === null ? [] : node.getSuggestions());
    }

    return result;
  }
}
```

