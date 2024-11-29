# 127. [Word Ladder](https://leetcode.com/problems/word-ladder/)

## Approach 1: Breadth-First Search (BFS)

### Solution
java
```java
// Time Complexity: O(n * m^2), where n is the number of words and m is the length of each word
// Space Complexity: O(n * m), for queue and visited set
import java.util.*;

public class Solution {
    public int ladderLength(String beginWord, String endWord, List<String> wordList) {
        Set<String> wordSet = new HashSet<>(wordList);
        if (!wordSet.contains(endWord)) return 0;

        Queue<String> queue = new LinkedList<>();
        queue.add(beginWord);
        int level = 1;

        while (!queue.isEmpty()) {
            int levelSize = queue.size();
            for (int i = 0; i < levelSize; i++) {
                String word = queue.poll();
                if (word.equals(endWord)) return level;

                // Try all possible one-letter transformations
                char[] wordChars = word.toCharArray();
                for (int j = 0; j < wordChars.length; j++) {
                    char originalChar = wordChars[j];
                    for (char c = 'a'; c <= 'z'; c++) {
                        if (wordChars[j] == c) continue;
                        wordChars[j] = c;
                        String newWord = new String(wordChars);
                        if (wordSet.contains(newWord)) {
                            queue.add(newWord);
                            wordSet.remove(newWord);
                        }
                    }
                    wordChars[j] = originalChar;
                }
            }
            level++;
        }

        return 0;
    }
}
```

typescript
```typescript
// Time Complexity: O(n * m^2), where n is the number of words and m is the length of each word
// Space Complexity: O(n * m), for queue and visited set
function ladderLength(beginWord: string, endWord: string, wordList: string[]): number {
    const wordSet = new Set(wordList);
    if (!wordSet.has(endWord)) return 0;

    const queue: string[] = [];
    queue.push(beginWord);
    let level = 1;

    while (queue.length > 0) {
        const levelSize = queue.length;
        for (let i = 0; i < levelSize; i++) {
            const word = queue.shift()!;
            if (word === endWord) return level;

            // Try all possible one-letter transformations
            const wordChars = word.split('');
            for (let j = 0; j < wordChars.length; j++) {
                const originalChar = wordChars[j];
                for (let c = 'a'; c <= 'z'; c++) {
                    if (wordChars[j] === c) continue;
                    wordChars[j] = c;
                    const newWord = wordChars.join('');
                    if (wordSet.has(newWord)) {
                        queue.push(newWord);
                        wordSet.delete(newWord);
                    }
                }
                wordChars[j] = originalChar;
            }
        }
        level++;
    }

    return 0;
}
```

