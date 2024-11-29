# 127. [Word Ladder](https://leetcode.com/problems/word-ladder/)

## Approach 1: Breadth-First Search (BFS)

### Solution
java
```java
// Time Complexity: O(M^2 * N), where M is the length of words and N is the total number of words in the word list.
// Space Complexity: O(M * N)
import java.util.*;

public class Solution {
    public int ladderLength(String beginWord, String endWord, List<String> wordList) {
        Set<String> wordSet = new HashSet<>(wordList);
        if (!wordSet.contains(endWord)) return 0;
        
        Queue<String> queue = new LinkedList<>();
        queue.add(beginWord);
        
        int level = 1; // Initial level is the beginWord itself
        
        while (!queue.isEmpty()) {
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                String currentWord = queue.poll();
                char[] currentWordChars = currentWord.toCharArray();
                
                for (int j = 0; j < currentWordChars.length; j++) {
                    char originalChar = currentWordChars[j];
                    
                    // Try all possibilities for current position
                    for (char c = 'a'; c <= 'z'; c++) {
                        currentWordChars[j] = c;
                        String transformedWord = new String(currentWordChars);
                        
                        if (transformedWord.equals(endWord)) {
                            return level + 1;
                        }
                        
                        if (wordSet.contains(transformedWord)) {
                            queue.add(transformedWord);
                            wordSet.remove(transformedWord);
                        }
                    }
                    
                    // Restore the character
                    currentWordChars[j] = originalChar;
                }
            }
            
            level++;
        }
        
        return 0;
    }
}
```

### Solution
javascript
```javascript
// Time Complexity: O(M^2 * N), where M is the length of words and N is the total number of words in the word list.
// Space Complexity: O(M * N)

function ladderLength(beginWord, endWord, wordList) {
    const wordSet = new Set(wordList);
    if (!wordSet.has(endWord)) return 0;
    
    const queue = [];
    queue.push(beginWord);
    
    let level = 1; // Initial level is the beginWord itself
    
    while (queue.length > 0) {
        const size = queue.length;
        for (let i = 0; i < size; i++) {
            const currentWord = queue.shift();
            const currentWordChars = currentWord.split('');
            
            for (let j = 0; j < currentWordChars.length; j++) {
                const originalChar = currentWordChars[j];
                
                // Try all possibilities for current position
                for (let c = 97; c <= 122; c++) { // 'a' to 'z'
                    currentWordChars[j] = String.fromCharCode(c);
                    const transformedWord = currentWordChars.join('');
                    
                    if (transformedWord === endWord) {
                        return level + 1;
                    }
                    
                    if (wordSet.has(transformedWord)) {
                        queue.push(transformedWord);
                        wordSet.delete(transformedWord);
                    }
                }
                
                // Restore the character
                currentWordChars[j] = originalChar;
            }
        }
        
        level++;
    }
    
    return 0;
}
```

