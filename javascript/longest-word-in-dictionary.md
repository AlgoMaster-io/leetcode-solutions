# [Leetcode 720: Longest Word in Dictionary](https://leetcode.com/problems/longest-word-in-dictionary/)

## Approaches
1. [Brute Force](#brute-force)
2. [Trie with Depth-First Search (DFS)](#trie-with-depth-first-search-dfs)
3. [Optimized Trie with BFS](#optimized-trie-with-bfs)

---

### Brute Force

**Intuition:**

The basic idea is to iterate through every word in the list, and for each word, check if by stripping one character at a time, it can still be constructed using other words in the list. We'll use a set for O(1) average time complexity when checking existence.

**Approach:**

1. Sort the words in lexicographical order to ensure that in case of a tie (i.e., words of the same length), the lexicographically smallest word is selected.
2. Use a set to store all the words.
3. For each word, check for each prefix by removing the last character repeatedly.
4. Check if all these prefixes are present in the word set.
5. If all prefixes are found, keep track of the longest word.

```javascript
var longestWord = function(words) {
    // Sort the words to ensure lexicographical order is maintained in case of a tie
    words.sort();
    let wordSet = new Set(words);
    let longest = "";
    
    for (let word of words) {
        // Check if every prefix of the word exists in the set
        let valid = true;
        for (let i = 1; i < word.length; i++) {
            if (!wordSet.has(word.substring(0, i))) {
                valid = false;
                break;
            }
        }
        // If valid and it's longer than the current longest, update
        if (valid && word.length > longest.length) {
            longest = word;
        }
    }
    
    return longest;
};
```

**Time Complexity:** O(N log N + N * M), where N is the number of words and M is the average length of words.  
**Space Complexity:** O(N) due to the storage in the set.

---

### Trie with Depth-First Search (DFS)

**Intuition:**

Constructing a Trie data structure allows efficient access to prefixes. We can insert all words into a Trie and then perform a DFS to find the longest word where every character can build off the previous one in sequence.

**Approach:**

1. Construct a Trie where each node stores a `Map` of children and a boolean indicating if the node marks the end of a valid word.
2. Insert words into the Trie.
3. Perform a DFS on the Trie to find the longest word such that every character builds off previous characters.

```javascript
class TrieNode {
    constructor() {
        this.children = new Map();
        this.isEndOfWord = false;
    }
}

var longestWord = function(words) {
    const root = new TrieNode();
    
    // Insert words into Trie
    for (const word of words) {
        let currentNode = root;
        for (const char of word) {
            if (!currentNode.children.has(char)) {
                currentNode.children.set(char, new TrieNode());
            }
            currentNode = currentNode.children.get(char);
        }
        currentNode.isEndOfWord = true;
    }

    let longest = '';
    
    // Helper function for DFS
    const dfs = (node, path) => {
        if (node.isEndOfWord) {
            if (path.length > longest.length) {
                longest = path;
            }
        }
        for (const [c, childNode] of node.children.entries()) {
            if (childNode.isEndOfWord) {
                dfs(childNode, path + c);
            }
        }
    };

    // Start DFS with an empty path
    for (const char of root.children.keys()) {
        const childNode = root.children.get(char);
        if (childNode.isEndOfWord) {
            dfs(childNode, char);
        }
    }

    return longest;
};
```

**Time Complexity:** O(N * M), where N is the number of words and M is the average length of words.  
**Space Complexity:** O(N * M) due to the storage in the Trie.

---

### Optimized Trie with BFS

**Intuition:**

Using a breadth-first search allows us to process each word once the longest chain is completed, leading to immediate processing as we discover longer words that build upon shorter, valid ones already processed.

**Approach:**

1. Insert words into a Trie.
2. Use a queue to implement BFS, starting from the shortest words.
3. Keep appending valid characters forming words in the dictionary to the queue.
4. Once the queue is exhausted, the longest valid path gives the desired word.

```javascript
class TrieNode {
    constructor() {
        this.children = new Map();
        this.isEndOfWord = false;
    }
}

var longestWord = function(words) {
    const root = new TrieNode();

    // Insert words into Trie
    for (const word of words) {
        let currentNode = root;
        for (const char of word) {
            if (!currentNode.children.has(char)) {
                currentNode.children.set(char, new TrieNode());
            }
            currentNode = currentNode.children.get(char);
        }
        currentNode.isEndOfWord = true;
    }

    let longest = '';
    const queue = [[root, '']];

    // Perform BFS
    while (queue.length > 0) {
        const [node, path] = queue.shift();
        for (const [c, childNode] of node.children.entries()) {
            if (childNode.isEndOfWord) {
                const newPath = path + c;
                if (newPath.length > longest.length) {
                    longest = newPath;
                }
                queue.push([childNode, newPath]);
            }
        }
    }

    return longest;
};
```

**Time Complexity:** O(N * M), where N is the number of words and M is the average length of words since we potentially process each node in the Trie at least once in the worst case.  
**Space Complexity:** O(N * M) due to the storage in the Trie.

