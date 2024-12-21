# [Leetcode 347: Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/)

## Approaches:
1. [Naive Approach: Sorting with a Frequency Map](#naive-approach-sorting-with-a-frequency-map)
2. [Optimized Approach: Using a Min-Heap (Priority Queue)](#optimized-approach-using-a-min-heap-priority-queue)
3. [Optimal Approach: Bucket Sort](#optimal-approach-bucket-sort)

---

### Naive Approach: Sorting with a Frequency Map

**Intuition:**

The problem is about finding the k most frequent elements in an array. A straightforward way to do this is to count the frequency of each number in the array and then sort the numbers based on their frequencies. This method makes use of a hashmap to store the frequencies and a sorting algorithm to find the elements with the highest frequencies.

**Steps:**
1. Use a hashmap to count the occurrences of each element.
2. Convert the hashmap into a vector of pairs (element, frequency).
3. Sort the vector by frequency in descending order.
4. Extract the top k elements from the sorted vector.

**Implementation:**

```cpp
#include <vector>
#include <unordered_map>
#include <algorithm>

std::vector<int> topKFrequent(std::vector<int>& nums, int k) {
    // Step 1: Use a hashmap to count the frequency of each element.
    std::unordered_map<int, int> frequencyMap;
    for (int num : nums) {
        frequencyMap[num]++;
    }

    // Step 2: Convert frequencyMap to a vector of pairs.
    std::vector<std::pair<int, int>> freqVec(frequencyMap.begin(), frequencyMap.end());

    // Step 3: Sort the vector by frequency in descending order.
    std::sort(freqVec.begin(), freqVec.end(), [](const std::pair<int, int>& a, const std::pair<int, int>& b) {
        return a.second > b.second;
    });

    // Step 4: Extract the top k elements.
    std::vector<int> result;
    for (int i = 0; i < k; ++i) {
        result.push_back(freqVec[i].first);
    }

    return result;
}
```

**Time Complexity:** O(N log N), where N is the number of elements in the list, due to the sorting step.

**Space Complexity:** O(N), the space used by the hashmap and the vector of pairs.

---

### Optimized Approach: Using a Min-Heap (Priority Queue)

**Intuition:**

Instead of sorting the entire list of frequencies, we can use a min-heap to keep track of the top k most frequent elements. This makes it unnecessary to sort all elements and allows us to efficiently maintain the top k elements.

**Steps:**
1. Count the frequency of each element using a hashmap.
2. Use a min-heap (priority queue) to keep the top k elements based on frequency.
3. As you iterate over the frequency map, maintain the heap so it holds at most k elements of maximum frequency.
4. Extract the elements from the heap.

**Implementation:**

```cpp
#include <vector>
#include <unordered_map>
#include <queue>

std::vector<int> topKFrequent(std::vector<int>& nums, int k) {
    // Step 1: Use a hashmap to count the frequency of each element.
    std::unordered_map<int, int> frequencyMap;
    for (int num : nums) {
        frequencyMap[num]++;
    }

    // Step 2: Use a min-heap to keep the top k frequent elements.
    std::priority_queue<std::pair<int, int>, std::vector<std::pair<int, int>>, std::greater<std::pair<int, int>>> minHeap;

    for (auto& [num, freq] : frequencyMap) {
        minHeap.push({freq, num});
        if (minHeap.size() > k) {
            minHeap.pop();
        }
    }

    // Step 3: Extract the elements from the heap.
    std::vector<int> result;
    while (!minHeap.empty()) {
        result.push_back(minHeap.top().second);
        minHeap.pop();
    }

    return result;
}
```

**Time Complexity:** O(N log k), where N is the number of unique elements. We maintain a heap of size k.

**Space Complexity:** O(N + k), where N is for the hashmap and k is for the min-heap.

---

### Optimal Approach: Bucket Sort

**Intuition:**

Another efficient approach is to use the concept of bucket sort. Since the number of possible frequencies is bound by the number of elements in the array, we can create buckets where each bucket corresponds to a frequency, and holds all numbers that are seen that, many times.

**Steps:**
1. Count the frequency of each element using a hashmap.
2. Use an array of vectors, `buckets`, where the index represents frequency and the list at that index contains all elements with that frequency.
3. Iterate over the buckets from end to start to collect the top k frequent elements.

**Implementation:**

```cpp
#include <vector>
#include <unordered_map>

std::vector<int> topKFrequent(std::vector<int>& nums, int k) {
    // Step 1: Use a hashmap to count the frequency of each element.
    std::unordered_map<int, int> frequencyMap;
    for (int num : nums) {
        frequencyMap[num]++;
    }
    
    // Step 2: Use an array of vectors as buckets.
    std::vector<std::vector<int>> buckets(nums.size() + 1);
    for (auto& [num, freq] : frequencyMap) {
        buckets[freq].push_back(num);
    }
    
    // Step 3: Collect the top k frequent elements.
    std::vector<int> result;
    for (int i = buckets.size() - 1; i >= 0 && result.size() < k; --i) {
        for (int num : buckets[i]) {
            result.push_back(num);
            if (result.size() == k) {
                break;
            }
        }
    }
    
    return result;
}
```

**Time Complexity:** O(N), where N is the number of elements in the input list. The bucketing approach is linear in terms of total number of operations relative to input size.

**Space Complexity:** O(N), due to storage used by the hashmap and buckets.

---

