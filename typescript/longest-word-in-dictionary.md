[Leetcode 720: Longest Word in Dictionary](https://leetcode.com/problems/longest-word-in-dictionary/)

## Approaches
1. [Brute-Force Solution](#brute-force-solution)
2. [Optimized Solution Using Trie](#optimized-solution-using-trie)

---

### Brute-Force Solution

#### Intuition:
The basic idea is to sort the words, for each word, verify if it can be built one character at a time by other words in the list. We perform a set of iterations to check the longest word which can be constructed from the sorted words. This solution is straightforward but inefficient.

#### Approach:
1. Sort the words in lexicographical order. This helps in automatically sorting by the longest word since for words of the same length, earlier in the list will be picked.
2. Use a set to keep track of words that can be built from scratch ('').
3. Iterate over sorted words and for each word:
   - Check if all its prefixes exist in the set.
   - If they do, this word becomes a candidate for the longest word, and it is added to the set.
4. Return the longest word found.

```typescript
function longestWord(words: string[]): string {
    // Sort words lexicographically
    words.sort();
    
    // Set to keep track of buildable words
    let wordSet = new Set<string>();
    let longestWord = "";

    // Start with an empty string which is buildable
    wordSet.add("");

    for (const word of words) {
        // Check if the word can be constructed by checking all prefixes
        if (wordSet.has(word.slice(0, -1))) {
            wordSet.add(word);
            // Update the longest word found
            if (word.length > longestWord.length) {
                longestWord = word;
            }
        }
    }

    return longestWord;
}
```

**Time Complexity**: O(N log N + N * L) - N words are sorted and for each word of average length L, we potentially perform a set operation.

**Space Complexity**: O(N * L) - We store N words, each with average length L.

---

### Optimized Solution Using Trie

#### Intuition:
Using a Trie, we can efficiently check the prefixes required to build a word. We insert each word into a Trie while also marking end of valid words. This helps in validating construction in constant time per word. We ensure that we only validate whole paths in the Trie that can be constructed step by step.

#### Approach:
1. Construct a Trie, where each node has a boolean flag indicating if the word ending at this node is valid.
2. Insert each word into the Trie and mark endings of valid words.
3. Traverse the Trie to find the longest word path where each node leading to it is a valid end.
4. Return the longest word recorded during the traversal.

```typescript
class TrieNode {
    children: Map<string, TrieNode>;
    isEndOfWord: boolean;

    constructor() {
        this.children = new Map();
        this.isEndOfWord = false;
    }
}

class Trie {
    root: TrieNode;

    constructor() {
        this.root = new TrieNode();
    }

    insert(word: string) {
        let node = this.root;
        for (const char of word) {
            if (!node.children.has(char)) {
                node.children.set(char, new TrieNode());
            }
            node = node.children.get(char)!;
        }
        node.isEndOfWord = true;
    }

    findLongestWord(): string {
        let longestWord = "";

        const dfs = (node: TrieNode, path: string) => {
            if (path.length > longestWord.length) {
                longestWord = path;
            }

            for (const [char, childNode] of node.children) {
                // Proceed only if childNode marks the end of a valid word
                if (childNode.isEndOfWord) {
                    dfs(childNode, path + char);
                }
            }
        }

        // Start DFS from root with an empty path
        node.children.forEach((childNode, char) => {
            if (childNode.isEndOfWord) {
                dfs(childNode, char);
            }
        });

        return longestWord;
    }
}

function longestWord(words: string[]): string {
    const trie = new Trie();
    for (const word of words) {
        trie.insert(word);
    }
    return trie.findLongestWord();
}
```

**Time Complexity**: O(N * L) - Each word of average length L is inserted and the Trie traversal visits each node once.

**Space Complexity**: O(N * L) - Trie size grows with each unique character path representing N words of average length L.

