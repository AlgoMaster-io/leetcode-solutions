# [451. Sort Characters By Frequency](https://leetcode.com/problems/sort-characters-by-frequency/)

## Approach 1: Using HashMap and Sorting

### Solution
```cpp
// Time Complexity: O(n log n)
// Space Complexity: O(n)
#include <unordered_map>
#include <queue>
#include <string>
#include <vector>

class Solution {
public:
    std::string frequencySort(std::string s) {
        // Count the frequency of each character
        std::unordered_map<char, int> frequencyMap;
        for (char c : s) {
            frequencyMap[c]++;
        }

        // Sort characters by frequency in descending order
        std::priority_queue<std::pair<int, char>> maxHeap;
        for (const auto& entry : frequencyMap) {
            maxHeap.push({entry.second, entry.first});
        }

        // Build the result string
        std::string result;
        while (!maxHeap.empty()) {
            auto entry = maxHeap.top();
            maxHeap.pop();
            result.append(entry.first, entry.second);
        }

        return result;
    }
};
```

## Approach 2: Bucket Sort (Optimized for Large Inputs)

### Solution
```cpp
// Time Complexity: O(n)
// Space Complexity: O(n)
#include <string>
#include <vector>
#include <unordered_map>

class Solution {
public:
    std::string frequencySort(std::string s) {
        // Count the frequency of each character
        std::vector<int> frequency(128, 0); // Assuming ASCII characters
        for (char c : s) {
            frequency[c]++;
        }

        // Create buckets where index represents frequency
        std::vector<std::vector<char>> buckets(s.size() + 1);
        for (int i = 0; i < 128; ++i) {
            if (frequency[i] > 0) {
                buckets[frequency[i]].push_back(static_cast<char>(i));
            }
        }

        // Build the result string from the buckets
        std::string result;
        for (int i = buckets.size() - 1; i > 0; --i) {
            for (char c : buckets[i]) {
                result.append(i, c);
            }
        }

        return result;
    }
};
```

