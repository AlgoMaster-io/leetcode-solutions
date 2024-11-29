# 127. [Word Ladder](https://leetcode.com/problems/word-ladder/)

## Approach 1: Bidirectional BFS

### Solution
java
```java
// Time Complexity: O(M^2 * N), where M is the length of each word, and N is the size of the wordList
// Space Complexity: O(M * N)
import java.util.*;

public class Solution {
    public int ladderLength(String beginWord, String endWord, List<String> wordList) {
        Set<String> wordSet = new HashSet<>(wordList);
        if (!wordSet.contains(endWord)) {
            return 0;
        }

        Queue<String> beginQueue = new LinkedList<>();
        Queue<String> endQueue = new LinkedList<>();
        beginQueue.add(beginWord);
        endQueue.add(endWord);
        
        Set<String> beginVisited = new HashSet<>();
        Set<String> endVisited = new HashSet<>();
        beginVisited.add(beginWord);
        endVisited.add(endWord);
        
        int step = 1;
        
        while (!beginQueue.isEmpty() && !endQueue.isEmpty()) {
            if (beginQueue.size() > endQueue.size()) {
                Queue<String> tempQueue = beginQueue;
                beginQueue = endQueue;
                endQueue = tempQueue;
                
                Set<String> tempVisited = beginVisited;
                beginVisited = endVisited;
                endVisited = tempVisited;
            }
            
            int size = beginQueue.size();
            for (int i = 0; i < size; i++) {
                String currentWord = beginQueue.poll();
                
                for (int j = 0; j < currentWord.length(); j++) {
                    char[] wordChars = currentWord.toCharArray();
                    for (char ch = 'a'; ch <= 'z'; ch++) {
                        wordChars[j] = ch;
                        String newWord = new String(wordChars);
                        
                        if (endVisited.contains(newWord)) {
                            return step + 1;
                        }
                        
                        if (!beginVisited.contains(newWord) && wordSet.contains(newWord)) {
                            beginVisited.add(newWord);
                            beginQueue.add(newWord);
                        }
                    }
                }
            }
            step++;
        }
        
        return 0;
    }
}
```

python
```python
# Time Complexity: O(M^2 * N), where M is the length of each word, and N is the size of the wordList
# Space Complexity: O(M * N)
from collections import deque

class Solution:
    def ladderLength(self, beginWord: str, endWord: str, wordList: List[str]) -> int:
        wordSet = set(wordList)
        if endWord not in wordSet:
            return 0

        beginQueue = deque([beginWord])
        endQueue = deque([endWord])
        
        beginVisited = set([beginWord])
        endVisited = set([endWord])
        
        step = 1
        
        while beginQueue and endQueue:
            if len(beginQueue) > len(endQueue):
                beginQueue, endQueue = endQueue, beginQueue
                beginVisited, endVisited = endVisited, beginVisited
            
            size = len(beginQueue)
            for _ in range(size):
                currentWord = beginQueue.popleft()
                
                for j in range(len(currentWord)):
                    wordChars = list(currentWord)
                    for ch in 'abcdefghijklmnopqrstuvwxyz':
                        wordChars[j] = ch
                        newWord = ''.join(wordChars)
                        
                        if newWord in endVisited:
                            return step + 1
                        
                        if newWord not in beginVisited and newWord in wordSet:
                            beginVisited.add(newWord)
                            beginQueue.append(newWord)
                    
            step += 1
        
        return 0
```

## Approach 2: Single-ended BFS

### Solution
java
```java
// Time Complexity: O(M^2 * N), where M is the length of each word, and N is the size of the wordList
// Space Complexity: O(M * N)
import java.util.*;

public class Solution {
    public int ladderLength(String beginWord, String endWord, List<String> wordList) {
        Set<String> wordSet = new HashSet<>(wordList);
        if (!wordSet.contains(endWord)) {
            return 0;
        }

        Queue<String> queue = new LinkedList<>();
        queue.add(beginWord);
        
        Set<String> visited = new HashSet<>();
        visited.add(beginWord);

        int step = 1;

        while (!queue.isEmpty()) {
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                String currentWord = queue.poll();
                for (int j = 0; j < currentWord.length(); j++) {
                    char[] wordChars = currentWord.toCharArray();
                    for (char ch = 'a'; ch <= 'z'; ch++) {
                        wordChars[j] = ch;
                        String newWord = new String(wordChars);

                        if (newWord.equals(endWord)) {
                            return step + 1;
                        }

                        if (!visited.contains(newWord) && wordSet.contains(newWord)) {
                            visited.add(newWord);
                            queue.add(newWord);
                        }
                    }
                }
            }
            step++;
        }

        return 0;
    }
}
```

python
```python
# Time Complexity: O(M^2 * N), where M is the length of each word, and N is the size of the wordList
# Space Complexity: O(M * N)
from collections import deque

class Solution:
    def ladderLength(self, beginWord: str, endWord: str, wordList: List[str]) -> int:
        wordSet = set(wordList)
        if endWord not in wordSet:
            return 0

        queue = deque([beginWord])
        
        visited = set([beginWord])

        step = 1

        while queue:
            size = len(queue)
            for _ in range(size):
                currentWord = queue.popleft()
                for j in range(len(currentWord)):
                    wordChars = list(currentWord)
                    for ch in 'abcdefghijklmnopqrstuvwxyz':
                        wordChars[j] = ch
                        newWord = ''.join(wordChars)

                        if newWord == endWord:
                            return step + 1

                        if newWord not in visited and newWord in wordSet:
                            visited.add(newWord)
                            queue.append(newWord)
                    
            step += 1
        
        return 0
```

