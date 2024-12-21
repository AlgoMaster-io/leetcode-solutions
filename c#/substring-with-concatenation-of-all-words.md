# [Leetcode 30: Substring with Concatenation of All Words](https://leetcode.com/problems/substring-with-concatenation-of-all-words/)

## Approaches
- [Brute Force Approach](#brute-force-approach)
- [Using Hash Map (Sliding Window)](#using-hash-map-sliding-window)

### Brute Force Approach

#### Intuition:
The problem is to find all starting indices of substring(s) in a given string `s` that is a concatenation of each word in `words` exactly once and without any intervening characters. A brute force approach would involve generating every possible substring of the size of all words combined and then checking if it is a valid concatenation.

#### Steps:
1. Calculate the total length of words combined.
2. Consider every substring of the same size in `s`.
3. Check if each substring is a valid concatenation by counting the words using a map and compare it to the frequency of words in `words`.

#### C# Code:

```csharp
public IList<int> FindSubstring(string s, string[] words) {
    IList<int> result = new List<int>();
    if (s == null || s.Length == 0 || words == null || words.Length == 0) return result;

    int wordLength = words[0].Length;
    int wordCount = words.Length;
    int substringLength = wordLength * wordCount;

    Dictionary<string, int> wordMap = new Dictionary<string, int>();
    foreach (string word in words) {
        if (wordMap.ContainsKey(word)) {
            wordMap[word]++;
        } else {
            wordMap[word] = 1;
        }
    }

    for (int i = 0; i <= s.Length - substringLength; i++) {
        Dictionary<string, int> currentMap = new Dictionary<string, int>(wordMap);
        int j;
        for (j = 0; j < wordCount; j++) {
            int wordIndex = i + j * wordLength;
            string currentWord = s.Substring(wordIndex, wordLength);
            if (currentMap.ContainsKey(currentWord)) {
                if (currentMap[currentWord] == 1) {
                    currentMap.Remove(currentWord);
                } else {
                    currentMap[currentWord]--;
                }
            } else {
                break;
            }
        }
        if (j == wordCount) {
            result.Add(i);
        }
    }
    return result;
}
```

#### Complexity Analysis:
- **Time Complexity**: O((N - L) * L) where N is the length of the string `s` and L is the length of the concatenated substring (word count * word length), as we are iterating indices and checking each substring.
- **Space Complexity**: O(W) where W is the number of unique words, as we store the frequency of words.

### Using Hash Map (Sliding Window)

#### Intuition:
To optimize the brute force solution, we can use the sliding window technique combined with hash maps. Instead of revalidating every substring from scratch, we can slide our window along the string, updating our checks incrementally.

#### Steps:
1. Use a double loop where the outer loop fixes the starting index for different segments of `s` based on word length.
2. Use a map to store the frequency of each word from `words`.
3. Use a sliding window to maintain the frequency of words in the current window of `s`.
4. Adjust the left and right pointers to maintain a valid window.

#### C# Code:

```csharp
public IList<int> FindSubstring(string s, string[] words) {
    IList<int> result = new List<int>();
    if (s == null || s.Length == 0 || words == null || words.Length == 0) return result;

    int wordLength = words[0].Length;
    int wordCount = words.Length;
    int substringLength = wordLength * wordCount;

    Dictionary<string, int> wordMap = new Dictionary<string, int>();
    foreach (string word in words) {
        if (wordMap.ContainsKey(word)) {
            wordMap[word]++;
        } else {
            wordMap[word] = 1;
        }
    }

    for (int i = 0; i < wordLength; i++) {
        int left = i, count = 0;
        Dictionary<string, int> currentMap = new Dictionary<string, int>();
        for (int j = i; j <= s.Length - wordLength; j += wordLength) {
            string word = s.Substring(j, wordLength);
            if (wordMap.ContainsKey(word)) {
                if (currentMap.ContainsKey(word)) {
                    currentMap[word]++;
                } else {
                    currentMap[word] = 1;
                }
                count++;

                while (currentMap[word] > wordMap[word]) {
                    string leftWord = s.Substring(left, wordLength);
                    currentMap[leftWord]--;
                    count--;
                    left += wordLength;
                }

                if (count == wordCount) {
                    result.Add(left);
                }
            } else {
                currentMap.Clear();
                count = 0;
                left = j + wordLength;
            }
        }
    }
    return result;
}
```

#### Complexity Analysis:
- **Time Complexity**: O(N * W) where N is the length of the string `s` and W is the number of words, due to the sliding window mechanism.
- **Space Complexity**: O(W) space for the map that stores the frequency of words.

This approach optimizes the brute force solution by effectively using the sliding window pattern to manage the index shifts and match checks incrementally.

