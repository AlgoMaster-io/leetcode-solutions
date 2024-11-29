# [767. Reorganize String](https://leetcode.com/problems/reorganize-string/)

## Approach 1: Max Heap

### Solution
```cpp
// Time Complexity: O(n log k), where k is the number of unique characters
// Space Complexity: O(n + k)
#include <vector>
#include <queue>
#include <string>

class Solution {
public:
    std::string reorganizeString(std::string s) {
        // Frequency map for characters
        std::vector<int> charCounts(26, 0);
        for (char c : s) {
            charCounts[c - 'a']++;
        }

        // Priority queue to keep the most frequent character at the top
        auto cmp = [](const std::pair<int, int>& a, const std::pair<int, int>& b) {
            return a.second < b.second;
        };
        std::priority_queue<std::pair<int, int>, std::vector<std::pair<int, int>>, decltype(cmp)> maxHeap(cmp);

        for (int i = 0; i < 26; ++i) {
            if (charCounts[i] > 0) {
                maxHeap.push({i, charCounts[i]});
            }
        }

        std::string result;
        std::pair<int, int> prev = {-1, 0}; // Previously used character and its count

        while (!maxHeap.empty()) {
            auto current = maxHeap.top();
            maxHeap.pop();
            result += (char)(current.first + 'a'); // Append current character
            current.second--; // Decrease its count

            // Re-add the previous character if it's still valid
            if (prev.second > 0) {
                maxHeap.push(prev);
            }

            // Update the previous character
            prev = current;
        }

        // If the result length is not the same as the input, return ""
        return result.size() == s.size() ? result : "";
    }
};
```

## Approach 2: Greedy with Frequency Array

### Solution
```cpp
// Time Complexity: O(n)
// Space Complexity: O(1) (assuming a fixed alphabet size)
#include <string>
#include <vector>

class Solution {
public:
    std::string reorganizeString(std::string s) {
        std::vector<int> charCounts(26, 0);
        int maxCount = 0;
        char maxChar = ' ';

        // Count frequencies and find the most frequent character
        for (char c : s) {
            charCounts[c - 'a']++;
            if (charCounts[c - 'a'] > maxCount) {
                maxCount = charCounts[c - 'a'];
                maxChar = c;
            }
        }

        // If the most frequent character cannot fit, return ""
        if (maxCount > (s.size() + 1) / 2) {
            return "";
        }

        std::string result(s.size(), ' ');
        int index = 0;

        // Place the most frequent character at even indices
        while (charCounts[maxChar - 'a'] > 0) {
            result[index] = maxChar;
            index += 2;
            charCounts[maxChar - 'a']--;
        }

        // Place the remaining characters
        for (int i = 0; i < 26; ++i) {
            while (charCounts[i] > 0) {
                if (index >= result.size()) {
                    index = 1; // Start filling odd indices
                }
                result[index] = (char)(i + 'a');
                index += 2;
                charCounts[i]--;
            }
        }

        return result;
    }
};
```

