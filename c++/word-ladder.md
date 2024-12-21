# [Leetcode 127: Word Ladder](https://leetcode.com/problems/word-ladder/)

## Table of Contents
- [Approach 1: BFS with Word List](#approach-1-bfs-with-word-list)
- [Approach 2: Bidirectional BFS](#approach-2-bidirectional-bfs)

### Approach 1: BFS with Word List

#### Intuition:
The problem of finding the shortest transformation sequence can be thought of as a graph traversal problem, where each word is a node and edges exist between nodes (words) that are one letter apart. The task is to find the shortest path from the start word to the target word, making it an ideal candidate for Breadth-First Search (BFS).

1. **Queue for BFS**: Use a queue to manage the current word and the number of steps taken to reach it.
2. **Visited Set**: Maintain a set to track words that have already been visited to avoid cycles.
3. **Word Transformation**: For each word, attempt to transform it by changing one letter at a time to see if it forms a valid word present in the word list.
4. **End Condition**: Once the end word is reached, return the number of steps taken.

#### Solution:

```cpp
#include <unordered_set>
#include <queue>
#include <string>
#include <vector>

class Solution {
public:
    int ladderLength(std::string beginWord, std::string endWord, std::vector<std::string>& wordList) {
        std::unordered_set<std::string> wordSet(wordList.begin(), wordList.end());

        // If the endWord is not in the list, there's no valid transformation.
        if (wordSet.find(endWord) == wordSet.end()) {
            return 0;
        }

        std::queue<std::pair<std::string, int>> bfsQueue;
        bfsQueue.push({beginWord, 1});

        while (!bfsQueue.empty()) {
            std::string currentWord = bfsQueue.front().first;
            int level = bfsQueue.front().second;
            bfsQueue.pop();

            // Try all possible single letter transformations
            for (int i = 0; i < currentWord.length(); ++i) {
                std::string transformedWord = currentWord;
                for (char ch = 'a'; ch <= 'z'; ++ch) {
                    transformedWord[i] = ch;
                    if (transformedWord == currentWord) continue;
                    if (transformedWord == endWord) return level + 1; // End word found

                    // Check if this transformed word is in the wordSet
                    if (wordSet.find(transformedWord) != wordSet.end()) {
                        bfsQueue.push({transformedWord, level + 1});
                        wordSet.erase(transformedWord); // Mark as visited
                    }
                }
            }
        }

        return 0; // No solution found
    }
};
```

#### Time Complexity:
- O(N \cdot M^2), where N is the length of `wordList` and M is the average length of words. The word transformation for a word of length M takes O(M^2) because, for each character, we have 26 possibilities. Combined with the number of words in the list, this results in O(N \cdot M^2).

#### Space Complexity:
- O(N \cdot M) for storing the word list in a set.

### Approach 2: Bidirectional BFS

#### Intuition:
Bidirectional BFS is a powerful variant of the standard BFS algorithm, which processes two search spaces simultaneously from both directions (begin and end). This approach reduces the search space significantly and is particularly beneficial in scenarios where the graph is large or like this problem, where the shortest path between two nodes in a dense graph is desirable.

1. **Two Queues**: One each for the forward and the reverse search.
2. **Meet in Middle**: Reduce search space because once both searches converge, the path length is minimal.
3. **Optimization by Swapping**: Always expand the smaller frontier first to keep the search balanced and efficient.

#### Solution:

```cpp
#include <unordered_set>
#include <queue>
#include <string>
#include <vector>

class Solution {
public:
    int ladderLength(std::string beginWord, std::string endWord, std::vector<std::string>& wordList) {
        std::unordered_set<std::string> wordSet(wordList.begin(), wordList.end());
        if (wordSet.find(endWord) == wordSet.end()) return 0;
        
        std::unordered_set<std::string> beginSet, endSet, visitedSet;
        beginSet.insert(beginWord);
        endSet.insert(endWord);
        int level = 1;

        while (!beginSet.empty() && !endSet.empty()) {
            // Always expand the smaller set for optimization.
            if (beginSet.size() > endSet.size()) {
                std::swap(beginSet, endSet);
            }

            std::unordered_set<std::string> nextLevel;
            for (std::string word : beginSet) {
                std::string transformedWord = word;
                for (int i = 0; i < transformedWord.length(); i++) {
                    char originalChar = transformedWord[i];
                    for (char ch = 'a'; ch <= 'z'; ch++) {
                        transformedWord[i] = ch;
                        if (endSet.find(transformedWord) != endSet.end()) {
                            return level + 1; // Path found
                        }

                        if (wordSet.find(transformedWord) != wordSet.end() && visitedSet.find(transformedWord) == visitedSet.end()) {
                            nextLevel.insert(transformedWord);
                            visitedSet.insert(transformedWord); // Mark visited
                        }
                    }
                    transformedWord[i] = originalChar;
                }
            }
            std::swap(beginSet, nextLevel);
            level++;
        }

        return 0;
    }
};
```

#### Time Complexity:
- O(N \cdot M), where N is the length of `wordList` and M is the average length of words. The search from both directions cuts down the required exploration to about half.

#### Space Complexity:
- O(N \cdot M) for storing the sets and visited nodes. 

In both approaches, BFS efficiently explores the transformation sequences, but the bidirectional BFS generally performs better for larger datasets due to its reduced search space.

