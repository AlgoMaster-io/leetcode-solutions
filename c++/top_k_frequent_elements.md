# 347. [Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/)

## Approach 1: Min-Heap

### Solution
```cpp
// Time Complexity: O(n * log(k)), where n is the number of elements in nums
// Space Complexity: O(n), for the frequency map and heap
#include <vector>
#include <unordered_map>
#include <queue>
#include <utility>

class Solution {
public:
    std::vector<int> topKFrequent(std::vector<int>& nums, int k) {
        // Step 1: Build a frequency map
        std::unordered_map<int, int> frequencyMap;
        for (int num : nums) {
            frequencyMap[num]++;
        }

        // Step 2: Use a min-heap to keep track of the top k elements
        auto comp = [](const std::pair<int, int>& a, const std::pair<int, int>& b) {
            return a.second > b.second;
        };
        std::priority_queue<std::pair<int, int>, std::vector<std::pair<int, int>>, decltype(comp)> minHeap(comp);

        for (const auto& entry : frequencyMap) {
            minHeap.emplace(entry.first, entry.second);

            if (minHeap.size() > k) {
                minHeap.pop(); // Remove the element with the smallest frequency
            }
        }

        // Step 3: Extract elements from the heap
        std::vector<int> result(k);
        for (int i = 0; i < k; i++) {
            result[i] = minHeap.top().first;
            minHeap.pop();
        }

        return result;
    }
};
```

## Approach 2: Bucket Sort

### Solution
```cpp
// Time Complexity: O(n), where n is the number of elements in nums
// Space Complexity: O(n), for the frequency map and buckets
#include <vector>
#include <unordered_map>
#include <list>

class Solution {
public:
    std::vector<int> topKFrequent(std::vector<int>& nums, int k) {
        // Step 1: Build a frequency map
        std::unordered_map<int, int> frequencyMap;
        for (int num : nums) {
            frequencyMap[num]++;
        }

        // Step 2: Create buckets where index represents frequency
        std::vector<std::list<int>> buckets(nums.size() + 1);
        for (const auto& entry : frequencyMap) {
            buckets[entry.second].push_back(entry.first);
        }

        // Step 3: Collect the top k frequent elements
        std::vector<int> result;
        for (int i = buckets.size() - 1; i >= 0 && result.size() < k; i--) {
            for (int num : buckets[i]) {
                result.push_back(num);
                if (result.size() == k) {
                    break;
                }
            }
        }

        return result;
    }
};
```

