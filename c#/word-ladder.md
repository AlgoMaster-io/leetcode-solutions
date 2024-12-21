# Leetcode Problem - Word Ladder

[Leetcode Problem 127: Word Ladder](https://leetcode.com/problems/word-ladder/)

## Approaches

1. [Breadth-First Search (BFS) with Transformation Check](#bfs-with-transformation-check)
2. [Optimized BFS with Preprocessed All Possible Intermediate Words](#optimized-bfs)

---

### BFS with Transformation Check

This approach utilizes a classic BFS to explore possible word transformations level by level. At each step, we replace every letter of the current word with every other letter from 'a' to 'z' and check whether we can transform the current word to a new word in the word list.

#### Intuition

- We treat each word in the word list as a node and the transformation between them as an edge.
- Use BFS to find the shortest path from `beginWord` to `endWord`.
- For each word, generate all possible next words by changing one character. If the new word is in the word list and has not been visited, add it to the BFS queue.

#### Time Complexity

- **O(M^2 \times N)** where M is the length of each word and N is the total number of words in the input list.

#### Space Complexity

- **O(N \times M)** to store the word list in a HashSet.

```csharp
public int LadderLength(string beginWord, string endWord, IList<string> wordList) {
    HashSet<string> wordSet = new HashSet<string>(wordList);
    if (!wordSet.Contains(endWord)) return 0;

    Queue<string> queue = new Queue<string>();
    queue.Enqueue(beginWord);
    int level = 1;

    while (queue.Count > 0) {
        int levelSize = queue.Count;
        for (int i = 0; i < levelSize; ++i) {
            string currentWord = queue.Dequeue();
            
            // Try changing each character
            for (int j = 0; j < currentWord.Length; ++j) {
                char[] currentWordArr = currentWord.ToCharArray();
                
                // Swap each character
                for (char c = 'a'; c <= 'z'; ++c) {
                    if (currentWordArr[j] == c) continue;
                    
                    currentWordArr[j] = c;
                    string newWord = new string(currentWordArr);
                    
                    // Check if the new word is our target
                    if (newWord.Equals(endWord)) return level + 1;
                    
                    // Otherwise, check if it's in wordList
                    if (wordSet.Contains(newWord)) {
                        wordSet.Remove(newWord);
                        queue.Enqueue(newWord);
                    }
                }
            }
        }
        level++;
    }
    return 0;
}
```

---

### Optimized BFS

In this approach, we preprocess all possible states before starting our BFS to enhance the efficiency of exploring states.

#### Intuition

- Preprocess the word list to create a mapping of intermediate words. For example, for a word `hit`, its intermediate states could be `*it`, `h*t`, `hi*`.
- Use BFS to process words, leveraging preprocessed mappings to reduce the number of transformations checked directly.
- This ensures reduced checks, as it provides direct access to potential next words.

#### Time Complexity

- **O(M^2 \times N)**, similar to the basic BFS approach since we preprocess in similar levels of complexity.

#### Space Complexity

- **O(M^2 \times N)** to store the processed state intermediate words.

```csharp
public int LadderLength(string beginWord, string endWord, IList<string> wordList) {
    if (!wordList.Contains(endWord)) return 0;

    // Preprocess the word list into a dictionary of intermediate words
    Dictionary<string, List<string>> allComboDict = new Dictionary<string, List<string>>();
    int L = beginWord.Length;

    foreach (string word in wordList) {
        for (int i = 0; i < L; i++) {
            string newWord = word.Substring(0, i) + '*' + word.Substring(i + 1, L - i - 1);
            if (!allComboDict.ContainsKey(newWord)) {
                allComboDict[newWord] = new List<string>();
            }
            allComboDict[newWord].Add(word);
        }
    }

    // BFS
    Queue<KeyValuePair<string, int>> queue = new Queue<KeyValuePair<string, int>>();
    queue.Enqueue(new KeyValuePair<string, int>(beginWord, 1));

    HashSet<string> visited = new HashSet<string>();
    visited.Add(beginWord);

    while (queue.Count > 0) {
        var node = queue.Dequeue();
        string word = node.Key;
        int level = node.Value;

        for (int i = 0; i < L; i++) {
            // Intermediate words for current word
            string newWord = word.Substring(0, i) + '*' + word.Substring(i + 1, L - i - 1);

            // Next states are all the words sharing the same intermediate state 
            if (allComboDict.ContainsKey(newWord)) {
                foreach (string adjacentWord in allComboDict[newWord]) {
                    // If we find the end word return the answer
                    if (adjacentWord.Equals(endWord)) {
                        return level + 1;
                    }

                    // Otherwise, add it to the BFS queue
                    if (!visited.Contains(adjacentWord)) {
                        visited.Add(adjacentWord);
                        queue.Enqueue(new KeyValuePair<string, int>(adjacentWord, level + 1));
                    }
                }
            }
        }
    }
    return 0;
}
```

Each code solution is provided with comprehensive comments to ensure clarity in what each part does and the associated complexities to help balance choice versus performance.

