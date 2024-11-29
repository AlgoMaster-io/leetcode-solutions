# 208. [Implement Trie (Prefix Tree)](https://leetcode.com/problems/implement-trie-prefix-tree/)

## Approach: Trie Node Implementation

### Solution

typescript
```typescript
// Time Complexity:
//   - insert(): O(m), where m is the length of the word
//   - search(): O(m), where m is the length of the word
//   - startsWith(): O(m), where m is the length of the prefix
// Space Complexity: O(n * m), where n is the number of words and m is the average word length

class Trie {
    private root: TrieNode;

    constructor() {
        this.root = new TrieNode(); // Initialize the root node
    }

    // Inserts a word into the trie
    insert(word: string): void {
        let node = this.root;
        for (const c of word) {
            if (!node.containsKey(c)) {
                node.put(c, new TrieNode()); // Create a new node if not already present
            }
            node = node.get(c); // Move to the next node
        }
        node.setEnd(); // Mark the end of the word
    }

    // Returns true if the word is in the trie
    search(word: string): boolean {
        const node = this.searchPrefix(word);
        return node !== null && node.isEnd(); // Check if the word ends at this node
    }

    // Returns true if there is any word in the trie that starts with the given prefix
    startsWith(prefix: string): boolean {
        return this.searchPrefix(prefix) !== null;
    }

    // Helper function to search for a node matching the prefix
    private searchPrefix(prefix: string): TrieNode | null {
        let node = this.root;
        for (const c of prefix) {
            if (node.containsKey(c)) {
                node = node.get(c); // Move to the next node
            } else {
                return null; // Prefix not found
            }
        }
        return node; // Return the last node of the prefix
    }
}

// Trie Node Class
class TrieNode {
    private links: (TrieNode | null)[];
    private readonly R = 26; // Number of possible children ('a' to 'z')
    private isEnd: boolean;

    constructor() {
        this.links = new Array(this.R).fill(null);
        this.isEnd = false;
    }

    containsKey(c: string): boolean {
        return this.links[c.charCodeAt(0) - 'a'.charCodeAt(0)] !== null;
    }

    get(c: string): TrieNode | null {
        return this.links[c.charCodeAt(0) - 'a'.charCodeAt(0)];
    }

    put(c: string, node: TrieNode): void {
        this.links[c.charCodeAt(0) - 'a'.charCodeAt(0)] = node;
    }

    setEnd(): void {
        this.isEnd = true;
    }

    isEnd(): boolean {
        return this.isEnd;
    }
}
```

