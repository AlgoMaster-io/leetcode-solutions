# [Leetcode 127: Word Ladder](https://leetcode.com/problems/word-ladder/)

## Solutions
- [Approach 1: Breadth-First Search (BFS)](#solution-1)
- [Approach 2: Bidirectional Breadth-First Search (BFS)](#solution-2)

### Solution 1: Breadth-First Search (BFS)

**Intuition:**
The Word Ladder problem can be visualized as a graph where each word is a node and an edge exists between two nodes if they differ by exactly one character. The problem then boils down to finding the shortest path between the start node (`beginWord`) and the target node (`endWord`). BFS is a natural choice for shortest path problems in unweighted graphs.

1. Start from `beginWord` and explore all possible one-character transformations.
2. Use a queue to keep track of the current word and the level (or depth) of transformation.
3. If the `endWord` is reached, return the level.
4. If not, mark the word as visited and continue transforming to other words.

**Time Complexity:** O(M^2 * N), where M is the length of each word and N is the total number of words in the `wordList`.

**Space Complexity:** O(M * N) for the queue and visited set.

```javascript
function ladderLength(beginWord, endWord, wordList) {
    const wordSet = new Set(wordList);
    if (!wordSet.has(endWord)) return 0;
    
    const queue = [[beginWord, 1]]; // each element is [word, level]
    
    while (queue.length > 0) {
        const [word, level] = queue.shift();
        
        for (let i = 0; i < word.length; i++) {
            // Attempt to change each character
            for (let c = 0; c < 26; c++) {
                const char = String.fromCharCode(97 + c); // a-z
                if (char === word[i]) continue;
                
                const newWord = word.slice(0, i) + char + word.slice(i + 1);
                
                if (newWord === endWord) return level + 1;
                
                if (wordSet.has(newWord)) {
                    wordSet.delete(newWord);
                    queue.push([newWord, level + 1]);
                }
            }
        }
    }
    
    return 0;
}
```

### Solution 2: Bidirectional Breadth-First Search (BFS)

**Intuition:**
Bidirectional BFS can significantly reduce the search space by simultaneously searching from both the `beginWord` and `endWord`. The idea is to begin BFS from both ends, and stop when the two searches meet. This reduces the branching factor which may decrease the search space by half.

1. Begin BFS from both the `beginWord` and `endWord`.
2. Alternate between the two BFS searches, each expanding words one character apart.
3. If at any point, the words from the two searches overlap, it indicates the shortest transformation.

**Time Complexity:** O(M^2 * N / 2), similar to the previous solution but potentially faster due to bidirectional search.

**Space Complexity:** O(M * N) for the two queues and visited sets.

```javascript
function ladderLength(beginWord, endWord, wordList) {
    const wordSet = new Set(wordList);
    if (!wordSet.has(endWord)) return 0;
    
    let beginSet = new Set([beginWord]);
    let endSet = new Set([endWord]);
    let visited = new Set();
    let length = 1;
    
    while (beginSet.size > 0 && endSet.size > 0) {
        // Always expand the smaller set for efficiency
        if (beginSet.size > endSet.size) {
            [beginSet, endSet] = [endSet, beginSet];
        }
        
        const tempSet = new Set();
        
        for (let word of beginSet) {
            // Attempt to change each character
            for (let i = 0; i < word.length; i++) {
                for (let c = 0; c < 26; c++) {
                    const char = String.fromCharCode(97 + c);
                    if (char === word[i]) continue;
                    
                    const newWord = word.slice(0, i) + char + word.slice(i + 1);
                    
                    if (endSet.has(newWord)) return length + 1;
                    
                    if (wordSet.has(newWord) && !visited.has(newWord)) {
                        tempSet.add(newWord);
                        visited.add(newWord);
                    }
                }
            }
        }
        
        beginSet = tempSet;
        length++;
    }
    
    return 0;
}
```

In both approaches, we strive towards optimizing the search space by maintaining smaller sets for exploration and controlling the breadth of word transformations efficiently. Bidirectional search offers a potential advantage when a significant reduction in the branching factor can be achieved.

