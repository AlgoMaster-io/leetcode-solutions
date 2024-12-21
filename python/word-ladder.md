# [Leetcode 127: Word Ladder](https://leetcode.com/problems/word-ladder/)

### Approaches:
1. [Breadth-First Search (BFS) with Word Dictionary](#bfs-with-word-dictionary)
2. [Optimized Bidirectional BFS](#optimized-bidirectional-bfs)

---

## 1. BFS with Word Dictionary

**Intuition:**

We want to find the shortest transformation sequence from a start word to an end word by changing one letter at a time with all intermediate words being valid. Because we are interested in the shortest path, BFS is a natural fit as it explores all neighbors level by level.

**Approach:**

1. Use a queue to facilitate the BFS iteration starting with the `beginWord`.
2. Maintain a set of `wordList` to keep track of the available transformations and to check word validity efficiently.
3. For each word dequeued, iterate through each character position.
4. Try substituting each position with every letter from 'a' to 'z'.
5. If the new word is the `endWord`, return the current level plus one.
6. If the new word is valid and unvisited, add it to the queue and remove it from the word set.
7. Continue the process until the queue is empty.
8. Return 0 if no transformation sequence is possible.

```python
from collections import deque

def ladderLength(beginWord, endWord, wordList):
    wordSet = set(wordList)  # Use a set for O(1) look-up
    if endWord not in wordSet:
        return 0

    queue = deque([(beginWord, 1)])  # Start BFS with the beginWord
    
    while queue:
        current_word, level = queue.popleft()
        
        # Check all possible transformations
        for i in range(len(current_word)):
            for c in range(ord('a'), ord('z') + 1):
                next_word = current_word[:i] + chr(c) + current_word[i + 1:]
                
                if next_word == endWord:
                    return level + 1
                
                if next_word in wordSet:
                    wordSet.remove(next_word)  # Remove to mark visited
                    queue.append((next_word, level + 1))
                    
    return 0

# Time Complexity: O(M^2 * N), where M is the length of each word and N is the number of words in the wordList.
# Space Complexity: O(N), the size of the queue in the worst case.
```

---

## 2. Optimized Bidirectional BFS

**Intuition:**

Bidirectional search can significantly reduce the search space, as it grows from both ends towards the middle and is expected to meet somewhere midway, thus involving fewer nodes overall.

**Approach:**

1. Perform two BFS simultaneously, one starting from `beginWord` and another from `endWord`.
2. Always expand from the smaller front set, this reduces the complexity and converges faster.
3. For every word in the current level, generate all possible one-letter variations.
4. If any of these variations exist in the opposite search set, the path length is calculated by summing the levels of both ends.
5. If not, expand the frontier further.
6. Keep the `wordSet` updated to prevent visiting the same word again.
7. Return 0 if both BFS explorations get exhausted without meeting.

```python
def ladderLengthBidirectional(beginWord, endWord, wordList):
    wordSet = set(wordList)
    if endWord not in wordSet:
        return 0

    beginSet = {beginWord}
    endSet = {endWord}
    level = 1

    while beginSet and endSet:
        # Always expand the smaller set
        if len(beginSet) > len(endSet):
            beginSet, endSet = endSet, beginSet
        
        next_level_set = set()
        for word in beginSet:
            for i in range(len(word)):
                for c in range(ord('a'), ord('z') + 1):
                    next_word = word[:i] + chr(c) + word[i + 1:]

                    if next_word in endSet:
                        return level + 1

                    if next_word in wordSet:
                        next_level_set.add(next_word)
                        wordSet.remove(next_word)  # Mark as visited

        beginSet = next_level_set
        level += 1
    
    return 0

# Time Complexity: O(M^2 * N), similar to the unidirectional BFS but practically faster due to reduced search space.
# Space Complexity: O(N), the space used remains linear relative to the wordList size.
```

---
These implementations provide a progressive understanding and solution path using BFS and its optimized bidirectional version to solve the word ladder problem efficiently.

