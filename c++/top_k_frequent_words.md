# [692. Top K Frequent Words](https://leetcode.com/problems/top-k-frequent-words/)

## Approach 1: HashMap and Min-Heap

### Solution
```cpp
// Time Complexity: O(n log k)
// Space Complexity: O(n)
#include <vector>
#include <string>
#include <unordered_map>
#include <queue>
#include <algorithm>

using namespace std;

class Solution {
public:
    vector<string> topKFrequent(vector<string>& words, int k) {
        // Count the frequency of each word
        unordered_map<string, int> frequencyMap;
        for (const string& word : words) {
            frequencyMap[word]++;
        }

        // Use a min-heap to keep the top k frequent words
        auto comp = [&](const string& a, const string& b) {
            return frequencyMap[a] == frequencyMap[b] ? a > b : frequencyMap[a] < frequencyMap[b];
        };
        priority_queue<string, vector<string>, decltype(comp)> minHeap(comp);

        for (const auto& pair : frequencyMap) {
            minHeap.push(pair.first);
            if (minHeap.size() > k) {
                minHeap.pop();
            }
        }

        // Build the result list from the heap
        vector<string> result;
        while (!minHeap.empty()) {
            result.push_back(minHeap.top());
            minHeap.pop();
        }
        reverse(result.begin(), result.end()); // Reverse to get the most frequent words first

        return result;
    }
};
```

## Approach 2: HashMap and Sorting

### Solution
```cpp
// Time Complexity: O(n + k log k)
// Space Complexity: O(n)
#include <vector>
#include <string>
#include <unordered_map>
#include <algorithm>

using namespace std;

class Solution {
public:
    vector<string> topKFrequent(vector<string>& words, int k) {
        // Count the frequency of each word
        unordered_map<string, int> frequencyMap;
        for (const string& word : words) {
            frequencyMap[word]++;
        }

        // Sort the words by frequency and lexicographical order
        vector<string> candidates;
        for (const auto& pair : frequencyMap) {
            candidates.push_back(pair.first);
        }
        sort(candidates.begin(), candidates.end(), [&](const string& a, const string& b) {
            return frequencyMap[a] == frequencyMap[b] ? a < b : frequencyMap[a] > frequencyMap[b];
        });

        // Return the top k words
        return vector<string>(candidates.begin(), candidates.begin() + k);
    }
};
```

