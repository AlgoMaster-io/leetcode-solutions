# [211. Design Add and Search Words Data Structure](https://leetcode.com/problems/design-add-and-search-words-data-structure/)

## Approaches
1. [Basic Brute Force with Array](#approach-1-basic-brute-force-with-array)
2. [Trie Data Structure](#approach-2-trie-data-structure)

### Approach 1: Basic Brute Force with Array

**Intuition:**

In this approach, we'll maintain a list of all words that are added. Searching for a word will involve iterating through this list and performing character-by-character checks to handle wildcards. This is a straightforward approach that directly addresses the problem but may not be efficient for larger datasets.

**Steps:**
1. Implement the `addWord` method by simply pushing words into an array.
2. For `search`, iterate through each word in the array and compare character by character.
3. Handle the '.' character by allowing any character to match.
4. Return true if a match is found, else false.

```typescript
class WordDictionary {
    private words: string[];

    constructor() {
        this.words = [];
    }

    addWord(word: string): void {
        this.words.push(word);
    }

    search(word: string): boolean {
        for (const w of this.words) {
            if (this.isMatch(w, word)) {
                return true;
            }
        }
        return false;
    }

    private isMatch(word: string, pattern: string): boolean {
        if (word.length !== pattern.length) return false;
        for (let i = 0; i < word.length; i++) {
            if (pattern[i] !== '.' && word[i] !== pattern[i]) {
                return false;
            }
        }
        return true;
    }
}

// Time Complexity: O(n * m), where n is the number of words and m is the average length of words.
// Space Complexity: O(n * m) for storing words in the array.
```

### Approach 2: Trie Data Structure

**Intuition:**

The Trie (prefix tree) is a tree data structure commonly used for handling strings efficiently. Using a Trie allows us to structure our words in a parent-child relationship, which can greatly optimize the search operation, especially with support for wildcards.

**Steps:**
1. Use nodes consisting of 26 possible children corresponding to each lowercase letter.
2. Identify the end of words using a boolean flag.
3. During `addWord`, navigate or create the necessary nodes.
4. For `search`, recursively traverse paths to match the current character or, if a '.', attempt all possibilities.

```typescript
class TrieNode {
    children: { [key: string]: TrieNode };
    isEnd: boolean;

    constructor() {
        this.children = {};
        this.isEnd = false;
    }
}

class WordDictionary {
    private root: TrieNode;

    constructor() {
        this.root = new TrieNode();
    }

    addWord(word: string): void {
        let node = this.root;
        for (const char of word) {
            if (!(char in node.children)) {
                node.children[char] = new TrieNode();
            }
            node = node.children[char];
        }
        node.isEnd = true;
    }

    search(word: string): boolean {
        return this.searchHelper(word, 0, this.root);
    }

    private searchHelper(word: string, index: number, node: TrieNode): boolean {
        if (index === word.length) {
            return node.isEnd;
        }

        const char = word[index];
        if (char === '.') {
            for (const child in node.children) {
                if (this.searchHelper(word, index + 1, node.children[child])) {
                    return true;
                }
            }
            return false;
        } else {
            if (!(char in node.children)) {
                return false;
            }
            return this.searchHelper(word, index + 1, node.children[char]);
        }
    }
}

// Time Complexity: O(m), where m is the length of the word being added or searched.
// Space Complexity: O(n * m), where n is the number of words and m is the average length of words, due to Trie nodes.
```

This approach optimizes search time by narrowing down results rapidly, and by using a Trie structure, it efficiently handles the representation of a large dataset in terms of space.

