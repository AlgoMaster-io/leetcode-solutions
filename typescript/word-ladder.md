## [Leetcode 127: Word Ladder](https://leetcode.com/problems/word-ladder/)

### Approaches

1. [Breadth-First Search (BFS) with Word Transformations](#bfs-with-word-transformations)
2. [Bidirectional BFS](#bidirectional-bfs)

---

### 1. BFS with Word Transformations

#### Intuition:

To solve the Word Ladder problem, we aim to transform a `beginWord` into an `endWord` by changing one letter at a time, ensuring each intermediate word is in a given word list. Our goal is to find the shortest such transformation sequence.

**BFS (Breadth-First Search)** is a natural choice, as it explores all possibilities at the present "breadth" (word transformation depth) before moving onto words that are further transformations. 

#### Approach:

- Start with a queue initialized with the `beginWord`.
- Use a set to track all words seen so far to prevent cycles.
- For each word, try changing every single letter (from 'a' to 'z') and check if the newly formed word is in the `wordList`.
- If yes, add it to the queue and mark it as seen.
- Repeat until `endWord` is found or no more words are left in the queue.

#### Implementation:

```typescript
function ladderLength(beginWord: string, endWord: string, wordList: string[]): number {
    const wordSet = new Set(wordList);
    if (!wordSet.has(endWord)) return 0;

    const queue: [string, number][] = [[beginWord, 1]]; // [currentWord, transformationDepth]
    wordSet.delete(beginWord);

    while (queue.length > 0) {
        const [currentWord, depth] = queue.shift()!;

        // Try all transformations by changing each letter
        for (let i = 0; i < currentWord.length; i++) {
            const wordArr = currentWord.split('');
            for (let charCode = 97; charCode <= 122; charCode++) {
                wordArr[i] = String.fromCharCode(charCode);
                const newWord = wordArr.join('');
                
                if (newWord === endWord) return depth + 1;

                if (wordSet.has(newWord)) {
                    queue.push([newWord, depth + 1]);
                    wordSet.delete(newWord); // Mark as visited
                }
            }
        }
    }
    return 0; // No transformation sequence found
}
```

**Complexity:**

- **Time Complexity:** O(M^2 * N), where M is the length of a word and N is the number of words in the word list.
- **Space Complexity:** O(M * N), mainly due to the queue and set.

---

### 2. Bidirectional BFS

#### Intuition:

Bidirectional BFS is an optimization over the standard BFS approach. Instead of searching from `beginWord` to `endWord` using a single queue, we simultaneously search from `beginWord` and `endWord`. This effectively reduces the search space, potentially leading to quicker termination because the search trees meet in the middle.

#### Approach:

- Initialize two sets: one begins from the start and the other from the target.
- Alternate between expanding the sets: use the smaller set to "grow" the search to keep the operations minimal.
- If there's an intersection between any intermediate words in both sets, it implies a shortest path has been found.

#### Implementation:

```typescript
function ladderLengthBidirectionalBFS(beginWord: string, endWord: string, wordList: string[]): number {
    const wordSet = new Set(wordList);
    if (!wordSet.has(endWord)) return 0;

    let beginSet = new Set<string>([beginWord]);
    let endSet = new Set<string>([endWord]);
    let length = 1;

    while (beginSet.size > 0) {
        // Always expand the smaller set for optimized performance
        if (beginSet.size > endSet.size) {
            [beginSet, endSet] = [endSet, beginSet];
        }

        const tempSet = new Set<string>();
        for (const word of beginSet) {
            for (let i = 0; i < word.length; i++) {
                const wordArr = word.split('');
                for (let charCode = 97; charCode <= 122; charCode++) {
                    wordArr[i] = String.fromCharCode(charCode);
                    const newWord = wordArr.join('');

                    if (endSet.has(newWord)) return length + 1;

                    if (wordSet.has(newWord)) {
                        tempSet.add(newWord);
                        wordSet.delete(newWord);
                    }
                }
            }
        }

        beginSet = tempSet;
        length += 1;
    }
    return 0; // No connection found
}
```

**Complexity:**

- **Time Complexity:** O(M^2 * N), similar to standard BFS but generally performs faster due to reduced search space.
- **Space Complexity:** O(N), due to sets used to track the current level of words being visited.

