# [Leetcode 30: Substring with Concatenation of All Words](https://leetcode.com/problems/substring-with-concatenation-of-all-words/)

## Approaches:
1. [Brute Force Approach](#brute-force-approach)
2. [Optimized Sliding Window with Hashmap](#optimized-sliding-window-with-hashmap)

---

## Brute Force Approach

### Intuition:
The brute force approach involves checking every possible starting index in the string `s`. For each starting index, we check if a valid concatenation of all the words in `words` can be found starting from that index. We loop through all possible positions and for each position, we try to match the subsequence with a permutation of the `words` list.

### Approach:
1. Calculate the length of each word in `words` and the total number of characters needed.
2. For each possible starting index in `s` (from 0 to `s.length() - totalLength`):
    - Extract the substring of length equal to `totalLength`.
    - Split this substring into words of the given length and check if they match exactly with the words in `words` using a map to compare frequencies.
3. If they do match, add the starting index to the result.

### Code:
```cpp
#include <vector>
#include <string>
#include <unordered_map>

std::vector<int> findSubstring(std::string s, std::vector<std::string>& words) {
    std::vector<int> result;
    if (s.empty() || words.empty()) return result;

    int wordLength = words[0].length();
    int wordCount = words.size();
    int totalLength = wordLength * wordCount;
    std::unordered_map<std::string, int> wordMap;

    // Populate the map with the frequency of each word.
    for (const auto& word : words) {
        wordMap[word]++;
    }

    for (int i = 0; i <= static_cast<int>(s.length()) - totalLength; ++i) {
        std::unordered_map<std::string, int> seenWords;
        int j = 0;

        while (j < wordCount) {
            int wordIndex = i + j * wordLength;
            std::string word = s.substr(wordIndex, wordLength);

            // If the word is not in the map, break the loop.
            if (wordMap.find(word) == wordMap.end()) {
                break;
            }
            
            seenWords[word]++;
            
            // If the word frequency exceeds expected frequency, break the loop.
            if (seenWords[word] > wordMap[word]) {
                break;
            }

            j++;
        }

        if (j == wordCount) {
            result.push_back(i);
        }
    }

    return result;
}
```

### Complexity Analysis:
- **Time Complexity**: O((n - m * k) * m * k), where `n` is the length of `s`, `m` is the number of words, and `k` is the length of each word.
- **Space Complexity**: O((m + n)/k) for the hashmap storage for words.

---

## Optimized Sliding Window with Hashmap

### Intuition:
The optimized sliding window approach uses the same idea of matching substrings with a concatenation of all words but does so more efficiently by iterating over possible word starting positions and using a sliding window to avoid repeating work.

### Approach:
1. Divide the process by starting points (0, 1, ..., wordLength-1) to ensure word alignment.
2. Use two pointers, `left` and `right`, to maintain a window containing a valid concatenation.
3. Use hashmaps to track the word counts needed and seen so far.
4. Progress the window by adjusting `left` or `right` based on the matched word's frequency compared to its required frequency.

### Code:
```cpp
#include <vector>
#include <string>
#include <unordered_map>

std::vector<int> findSubstring(std::string s, std::vector<std::string>& words) {
    std::vector<int> result;
    if (s.empty() || words.empty()) return result;

    int wordLength = words[0].size();
    int wordCount = words.size();
    int totalLength = wordLength * wordCount;
    std::unordered_map<std::string, int> wordMap, currentMap;

    for (const auto& word : words) {
        wordMap[word]++;
    }

    for (int i = 0; i < wordLength; ++i) {
        int left = i, count = 0;
        currentMap.clear();

        for (int right = i; right <= static_cast<int>(s.size()) - wordLength; right += wordLength) {
            std::string word = s.substr(right, wordLength);

            if (wordMap.count(word)) {
                currentMap[word]++;
                count++;

                while (currentMap[word] > wordMap[word]) {
                    std::string leftWord = s.substr(left, wordLength);
                    currentMap[leftWord]--;
                    count--;
                    left += wordLength;
                }

                if (count == wordCount) {
                    result.push_back(left);
                }
            } else {
                currentMap.clear();
                count = 0;
                left = right + wordLength;
            }
        }
    }

    return result;
}
```

### Complexity Analysis:
- **Time Complexity**: O(n * k), where `n` is the length of `s`, and `k` is the word length. The sliding window ensures each character is processed at most twice.
- **Space Complexity**: O(m + n/k) due to the maps used for counts of required and current words. 

By iterating through word-aligned starting indices and effectively managing seen word counts, this optimized approach achieves significant efficiency over the brute force method.

