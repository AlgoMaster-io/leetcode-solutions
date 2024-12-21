[Leetcode 692: Top K Frequent Words](https://leetcode.com/problems/top-k-frequent-words/)

### Approaches
1. [Using HashMap and Sorting](#using-hashmap-and-sorting)
2. [Using Min-Heap](#using-min-heap)
3. [Using Trie and Bucket Sort](#using-trie-and-bucket-sort)

---

## Approach 1: Using HashMap and Sorting

### Intuition
We can count the frequency of each word using a hashmap. Once we have the frequency, we can sort the words based on their frequency, and resolve ties by sorting lexicographically. This is the most straightforward solution but might not be optimal when the number of words is very large and `k` is small.

### Steps
1. Use a hashmap to store the frequency of each word.
2. Sort the words first by frequency in descending order, and then lexicographically for those with equal frequency.
3. Return the first `k` words from the sorted list.

### Code
```cpp
#include <iostream>
#include <vector>
#include <unordered_map>
#include <string>
#include <algorithm>

std::vector<std::string> topKFrequent(std::vector<std::string>& words, int k) {
    // Step 1: Count frequency of each word
    std::unordered_map<std::string, int> frequencyMap;
    for (const auto& word : words) {
        frequencyMap[word]++;
    }
    
    // Step 2: Transfer to a vector for sorting
    std::vector<std::pair<std::string, int>> freqVec(frequencyMap.begin(), frequencyMap.end());
    
    // Step 3: Sort the vector based on custom comparator
    std::sort(freqVec.begin(), freqVec.end(), [](const std::pair<std::string, int>& a, const std::pair<std::string, int>& b) {
        return a.second > b.second || (a.second == b.second && a.first < b.first);
    });
    
    // Step 4: Gather the top k elements
    std::vector<std::string> result;
    for (int i = 0; i < k; ++i) {
        result.push_back(freqVec[i].first);
    }
    
    return result;
}
```

### Complexity
- **Time Complexity**: O(N log N), where N is the number of unique words. Sorting is the most time-consuming operation.
- **Space Complexity**: O(N), used for the frequency map and sorting vector.

---

## Approach 2: Using Min-Heap

### Intuition
Instead of sorting all unique words, we can maintain a min-heap of size `k` to keep the top `k` frequent words, reducing unnecessary sorting.

### Steps
1. Use a hashmap to store the frequency of each word.
2. Iterate over the frequency map and maintain a min-heap of size `k`.
3. Always maintain `k` top frequent elements in the heap. For the same frequency, use lexicographical order.
4. Extract the words from the min-heap.

### Code
```cpp
#include <iostream>
#include <vector>
#include <unordered_map>
#include <string>
#include <queue>

std::vector<std::string> topKFrequent(std::vector<std::string>& words, int k) {
    std::unordered_map<std::string, int> frequencyMap;
    for (const auto& word : words) {
        frequencyMap[word]++;
    }
    
    auto comp = [](const std::pair<int, std::string>& a, const std::pair<int, std::string>& b) {
        return a.first > b.first || (a.first == b.first && a.second < b.second);
    };
    
    std::priority_queue<std::pair<int, std::string>, std::vector<std::pair<int, std::string>>, decltype(comp)> minHeap(comp);
    
    for (const auto& [word, freq] : frequencyMap) {
        minHeap.emplace(freq, word);
        if (minHeap.size() > k) minHeap.pop();
    }
    
    std::vector<std::string> result;
    while (!minHeap.empty()) {
        result.push_back(minHeap.top().second);
        minHeap.pop();
    }
    
    std::reverse(result.begin(), result.end()); // Reverse to get top frequent elements in correct order
    return result;
}
```

### Complexity
- **Time Complexity**: O(N log k), where N is the number of unique words. We process each word in the map and maintain a heap of size `k`.
- **Space Complexity**: O(N + k), for the frequency map and the heap.

---

## Approach 3: Using Trie and Bucket Sort

### Intuition
This approach leverages the Trie data structure for efficient alphabetical ordering and then uses bucket sort for frequency sorting. It's optimal in cases with large datasets but is complex to implement.

### Steps
1. Build a Trie that keeps track of word frequencies.
2. Use bucket sort to sort words by frequency.
3. In the buckets, sort words lexicographically using the Trie information.

### Code
The implementation is omitted here due to complexity, but the approach involves modifying a classic Trie to include frequency counting and applying bucket sort for organizing by frequency, ensuring efficiency akin to counting sort.

### Complexity
- **Time Complexity**: O(N + W log W), where W is the unique number of words. Trie operations (insert and lexicographical traversal) commonly add logarithmic factors.
- **Space Complexity**: O(N + L), with L for the Trie storage, representing all characters in unique words.


