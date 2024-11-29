# 127. [Word Ladder](https://leetcode.com/problems/word-ladder/)

## Approach 1: Breadth-First Search (BFS)

### Solution
java
```java
// Time Complexity: O(m^2 * n), where m is the length of each word and n is the number of words in the wordList
// Space Complexity: O(m^2 * n)
import java.util.HashSet;
import java.util.LinkedList;
import java.util.Queue;
import java.util.List;

public class Solution {
    public int ladderLength(String beginWord, String endWord, List<String> wordList) {
        HashSet<String> wordSet = new HashSet<>(wordList);
        if (!wordSet.contains(endWord)) {
            return 0;
        }

        Queue<String> queue = new LinkedList<>();
        queue.add(beginWord);
        int level = 1;

        while (!queue.isEmpty()) {
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                String currentWord = queue.poll();
                char[] wordChars = currentWord.toCharArray();
                
                for (int j = 0; j < wordChars.length; j++) {
                    char originalChar = wordChars[j];
                    for (char c = 'a'; c <= 'z'; c++) {
                        if (c == originalChar) continue;
                        wordChars[j] = c;
                        String newWord = new String(wordChars);
                        if (newWord.equals(endWord)) {
                            return level + 1;
                        }
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

c++
```cpp
// Time Complexity: O(m^2 * n), where m is the length of each word and n is the number of words in the wordList
// Space Complexity: O(m^2 * n)
#include <unordered_set>
#include <queue>
#include <vector>
#include <string>

using namespace std;

class Solution {
public:
    int ladderLength(string beginWord, string endWord, vector<string>& wordList) {
        unordered_set<string> wordSet(wordList.begin(), wordList.end());
        if (wordSet.find(endWord) == wordSet.end()) {
            return 0;
        }

        queue<string> queue;
        queue.push(beginWord);
        int level = 1;

        while (!queue.empty()) {
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                string currentWord = queue.front();
                queue.pop();
                for (int j = 0; j < currentWord.length(); j++) {
                    char originalChar = currentWord[j];
                    for (char c = 'a'; c <= 'z'; c++) {
                        if (c == originalChar) continue;
                        currentWord[j] = c;
                        if (currentWord == endWord) {
                            return level + 1;
                        }
                        if (wordSet.find(currentWord) != wordSet.end()) {
                            queue.push(currentWord);
                            wordSet.erase(currentWord);
                        }
                    }
                    currentWord[j] = originalChar;
                }
            }
            level++;
        }

        return 0;
    }
};
```


