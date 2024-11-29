# 211. [Design Add and Search Words Data Structure](https://leetcode.com/problems/design-add-and-search-words-data-structure/)

## Approach: Trie with Backtracking for Search

### Solution
typescript
```typescript
// Time Complexity:
//   - addWord(): O(m), where m is the length of the word
//   - search(): O(m * 26^k), where m is the length of the word and k is the number of wildcards
// Space Complexity: O(n * m), where n is the number of words and m is the average word length

class WordDictionary {

    private root: TrieNode;

    constructor() {
        this.root = new TrieNode(); // Initialize the root node
    }

    // Adds a word into the data structure
    public addWord(word: string): void {
        let node = this.root;
        for (const c of word) {
            if (!node.containsKey(c)) {
                node.put(c, new TrieNode()); // Create a new node if not already present
            }
            node = node.get(c)!; // Move to the next node
        }
        node.setEnd(); // Mark the end of the word
    }

    // Returns true if the word is in the data structure, allowing '.' as a wildcard
    public search(word: string): boolean {
        return this.searchInNode(word, 0, this.root);
    }

    // Helper function for backtracking search
    private searchInNode(word: string, index: number, node: TrieNode | null): boolean {
        if (node === null) {
            return false; // Base case: node doesn't exist
        }
        if (index === word.length) {
            return node.isEnd(); // Check if the word ends at this node
        }

        const c = word[index];
        if (c === '.') {
            // If the current character is '.', check all possible children
            for (let nextChar = 'a'.charCodeAt(0); nextChar <= 'z'.charCodeAt(0); nextChar++) {
                if (this.searchInNode(word, index + 1, node.get(String.fromCharCode(nextChar)))) {
                    return true;
                }
            }
        } else {
            // If the current character is not '.', proceed with the specific child
            return this.searchInNode(word, index + 1, node.get(c));
        }
        return false; // No match found
    }
}

// Trie Node Class
class TrieNode {
    private links: Array<TrieNode | null>;
    private readonly R = 26; // Number of possible children ('a' to 'z')
    private isEnd: boolean;

    constructor() {
        this.links = new Array(this.R).fill(null);
        this.isEnd = false;
    }

    public containsKey(c: string): boolean {
        return this.links[c.charCodeAt(0) - 'a'.charCodeAt(0)] !== null;
    }

    public get(c: string): TrieNode | null {
        return this.links[c.charCodeAt(0) - 'a'.charCodeAt(0)];
    }

    public put(c: string, node: TrieNode): void {
        this.links[c.charCodeAt(0) - 'a'.charCodeAt(0)] = node;
    }

    public setEnd(): void {
        this.isEnd = true;
    }

    public isEnd(): boolean {
        return this.isEnd;
    }
}
```

