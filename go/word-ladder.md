# [Leetcode 127: Word Ladder](https://leetcode.com/problems/word-ladder/)

## Approaches
1. [Breadth-First Search (BFS) Approach](#approach-1-bfs)
2. [Bidirectional BFS Approach](#approach-2-bidirectional-bfs)

---

## Approach 1: BFS

### Intuition
The problem involves finding the shortest transformation sequence from the `beginWord` to `endWord`, where each intermediate word must exist in the given word list. The transformation can only change one character at a time. This scenario is perfect for BFS because we are interested in the shortest number of transformations - BFS naturally examines nodes layer-by-layer, ensuring the shortest path is found first.

### Steps
1. Use a queue to facilitate the BFS.
2. Start BFS from `beginWord`. Push it into the queue along with an initial transformation count of 1.
3. Use a set to track words that have been visited to avoid cycles.
4. For each word, try changing each character ('a' to 'z') and check if the modified word exists in the word list.
5. If a modified word equals `endWord`, return the transformation count incremented by one.
6. If the word exists in the word list and has not been visited, mark it as visited and add it to the queue with an incremented transformation count.
7. If the queue is exhausted without finding the `endWord`, return 0.

```go
package main

import (
	"container/list"
)

func ladderLength(beginWord string, endWord string, wordList []string) int {
	wordSet := make(map[string]bool)
	for _, word := range wordList {
		wordSet[word] = true
	}

	if !wordSet[endWord] {
		return 0
	}

	queue := list.New()
	queue.PushBack([]string{beginWord, "1"})

	for queue.Len() > 0 {
		current := queue.Remove(queue.Front()).([]string)
		word := current[0]
		step := current[1]

		for i := 0; i < len(word); i++ {
			for c := 'a'; c <= 'z'; c++ {
				if byte(c) == word[i] {
					continue
				}
				newWord := word[:i] + string(c) + word[i+1:]
				if newWord == endWord {
					return toInt(step) + 1
				}
				if wordSet[newWord] {
					delete(wordSet, newWord)
					queue.PushBack([]string{newWord, step + "1"})
				}
			}
		}
	}
	return 0
}

func toInt(s string) int {
	res := 0
	for _, v := range s {
		res = res*10 + int(v-'0')
	}
	return res
}
```

### Complexity Analysis
- **Time Complexity**: O(M^2 * N), where M is the length of each word and N is the total number of words in the `wordList`.
- **Space Complexity**: O(M * N), the space used by the queue and set.

---

## Approach 2: Bidirectional BFS

### Intuition
Bidirectional BFS optimizes the search by performing BFS from both the `beginWord` and `endWord`, meeting in the middle. This reduces the breadth of search significantly.

### Steps
1. Maintain two sets representing words discovered from both ends.
2. At each level, expand the smaller set.
3. Use a similar logic to BFS to explore transformations.
4. If the two sets intersect, return the cumulative transformation steps from both sides.
5. Progressively expand and swap the sets to always expand the smaller one for efficiency.

```go
package main

func ladderLengthBidirectional(beginWord string, endWord string, wordList []string) int {
	wordSet := make(map[string]bool)
	for _, word := range wordList {
		wordSet[word] = true
	}

	if !wordSet[endWord] {
		return 0
	}

	beginSet := make(map[string]bool)
	endSet := make(map[string]bool)
	beginSet[beginWord] = true
	endSet[endWord] = true

	step := 1

	for len(beginSet) > 0 && len(endSet) > 0 {
		if len(beginSet) > len(endSet) {
			beginSet, endSet = endSet, beginSet
		}

		nextSet := make(map[string]bool)
		for word := range beginSet {
			for i := 0; i < len(word); i++ {
				for c := 'a'; c <= 'z'; c++ {
					if byte(c) == word[i] {
						continue
					}
					newWord := word[:i] + string(c) + word[i+1:]
					if endSet[newWord] {
						return step + 1
					}
					if wordSet[newWord] {
						nextSet[newWord] = true
						delete(wordSet, newWord)
					}
				}
			}
		}
		beginSet = nextSet
		step++
	}

	return 0
}
```

### Complexity Analysis
- **Time Complexity**: O(M^2 * N), where M is the length of each word and N is the total number of words in the `wordList`.
- **Space Complexity**: O(M * N), just like the BFS, due to the sets used for storing words.

This approach, while having the same theoretical complexity, often performs better in practice due to processing fewer nodes by expanding from both directions.

