# 720. [Longest Word in Dictionary](https://leetcode.com/problems/longest-word-in-dictionary/)

## Approach 1: Sort and HashSet

### Solution
typescript
```typescript
// Time Complexity: O(n * k), where n is the number of words and k is the average length of a word
// Space Complexity: O(n * k)
function longestWord(words: string[]): string {
    words.sort(); // Sort words lexicographically
    const built = new Set<string>();
    let result = "";

    for (const word of words) {
        // Check if the prefix (word without the last character) is in the set
        if (word.length === 1 || built.has(word.substring(0, word.length - 1))) {
            built.add(word); // Add the word to the set
            // Update result if the word is longer or lexicographically smaller
            if (
                word.length > result.length || 
                (word.length === result.length && word.localeCompare(result) < 0)
            ) {
                result = word;
            }
        }
    }

    return result;
}
```

## Approach 2: Trie-Based Solution

### Solution
typescript
```typescript
// Time Complexity: O(n * k), where n is the number of words and k is the average length of a word
// Space Complexity: O(n * k)
interface TrieNode {
    children: Map<string, TrieNode>;
    isEndOfWord: boolean;
    word: string;
}

function createTrieNode(): TrieNode {
    return { children: new Map<string, TrieNode>(), isEndOfWord: false, word: "" };
}

function longestWord(words: string[]): string {
    const root = createTrieNode();

    // Build the Trie
    for (const word of words) {
        let current = root;
        for (const c of word) {
            if (!current.children.has(c)) {
                current.children.set(c, createTrieNode());
            }
            current = current.children.get(c)!;
        }
        current.isEndOfWord = true;
        current.word = word;
    }

    // Perform DFS to find the longest word
    return dfs(root, "");
}

function dfs(node: TrieNode, result: string): string {
    if (!node || (!node.isEndOfWord && node)) {
        return result;
    }

    // Check for the longest word in the current branch
    if (
        node.word.length > result.length || 
        (node.word.length === result.length && node.word.localeCompare(result) < 0)
    ) {
        result = node.word;
    }

    for (let c = 'a'; c <= 'z'; c++) {
        result = dfs(node.children.get(c)!, result);
    }

    return result;
}
```

