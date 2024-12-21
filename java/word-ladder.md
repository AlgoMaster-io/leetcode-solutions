# [Leetcode 127: Word Ladder](https://leetcode.com/problems/word-ladder/)

## Approaches
- [Approach 1: Breadth-First Search (BFS)](#approach-1-breadth-first-search-bfs)
- [Approach 2: Bidirectional BFS](#approach-2-bidirectional-bfs)

---

## Approach 1: Breadth-First Search (BFS)

### Intuition:
The Word Ladder problem can be thought of as a graph traversal problem where each word in the word list is a node and there is an edge between two nodes if they differ by exactly one character. The problem is then to find the shortest path from the `beginWord` to the `endWord`.

Breadth-First Search (BFS) is a perfect fit for this type of problem because it explores all nodes at the present "depth" level before moving on to nodes at the next depth level, thereby ensuring that we find the shortest path.

### Code:

```java
import java.util.*;

public class Solution {
    public int ladderLength(String beginWord, String endWord, List<String> wordList) {
        Set<String> wordSet = new HashSet<>(wordList); // Convert wordList to a set for O(1) access
        if (!wordSet.contains(endWord)) { // if endWord is not in the dictionary
            return 0;
        }

        Queue<String> queue = new LinkedList<>();
        queue.offer(beginWord);
        int level = 1; // Represents number of transformations

        while (!queue.isEmpty()) {
            int size = queue.size();
            
            // Process all nodes at the current depth level
            for (int i = 0; i < size; i++) {
                String currentWord = queue.poll();
                
                // Try changing each letter of the current word to find all possible transformations
                char[] wordArray = currentWord.toCharArray();
                for (int j = 0; j < wordArray.length; j++) {
                    char originalChar = wordArray[j];
                    for (char c = 'a'; c <= 'z'; c++) {
                        if (wordArray[j] == c) continue; // skip if the character is the same
                        wordArray[j] = c;
                        String newWord = new String(wordArray);

                        if (newWord.equals(endWord)) {
                            return level + 1; // Shortest path found
                        }

                        if (wordSet.contains(newWord)) {
                            queue.offer(newWord);
                            wordSet.remove(newWord); // Remove to prevent re-visiting
                        }
                    }
                    wordArray[j] = originalChar; // Restore original letter for next iteration
                }
            }
            level++; // Increment depth level
        }
        
        return 0; // If endWord is not reachable from beginWord
    }
}
```

### Time Complexity:
- O(N \* M \* 26) where N is the number of words in the word list and M is the length of each word. For every word, in the worst case, we change each character (M) to every other character in the alphabet (26 possibilities).

### Space Complexity:
- O(N \* M) for the wordSet and queue data structures.

---

## Approach 2: Bidirectional BFS

### Intuition:
Bidirectional BFS is an optimization of the traditional BFS technique. Instead of searching from one end to the other, we simultaneously search from both the `beginWord` and the `endWord` towards each other. The search halts when the two searches meet. This significantly reduces the search space and can greatly increase efficiency, especially as the length of transformation increases.

### Code:

```java
import java.util.*;

public class Solution {
    public int ladderLength(String beginWord, String endWord, List<String> wordList) {
        Set<String> wordSet = new HashSet<>(wordList);
        if (!wordSet.contains(endWord)) {
            return 0;
        }

        Set<String> beginSet = new HashSet<>(); // Set for one direction
        Set<String> endSet = new HashSet<>(); // Set for the reverse direction
        beginSet.add(beginWord);
        endSet.add(endWord);

        int level = 1;

        while (!beginSet.isEmpty() && !endSet.isEmpty()) {
            // Always iterate from the smaller set to optimize the search
            if (beginSet.size() > endSet.size()) {
                Set<String> temp = beginSet;
                beginSet = endSet;
                endSet = temp;
            }

            Set<String> nextSet = new HashSet<>();
            for (String word : beginSet) {
                char[] wordArray = word.toCharArray();
                
                for (int i = 0; i < wordArray.length; i++) {
                    char originalChar = wordArray[i];
                    for (char c = 'a'; c <= 'z'; c++) {
                        if (c == originalChar) continue;
                        wordArray[i] = c;
                        String newWord = new String(wordArray);

                        if (endSet.contains(newWord)) { // Bidirectional intersection
                            return level + 1;
                        }

                        if (wordSet.contains(newWord)) {
                            nextSet.add(newWord);
                            wordSet.remove(newWord);
                        }
                    }
                    wordArray[i] = originalChar;
                }
            }

            beginSet = nextSet; // Move to the next layer
            level++;
        }
        
        return 0;
    }
}
```

### Time Complexity:
- O(N \* M \* 26) similar to BFS. However, in practice, the performance can be significantly better since we are working on possibly halved problem size.

### Space Complexity:
- O(N \* M) for the word sets and additional data structures. However, it tends to require less space on average compared to the unidirectional BFS.

---

