# [Leetcode 451: Sort Characters By Frequency](https://leetcode.com/problems/sort-characters-by-frequency/)

## Approaches

1. [Basic Counting and Sorting](#basic-counting-and-sorting)
2. [Priority Queue (Heap)](#priority-queue-heap)
3. [Bucket Sort](#bucket-sort)

---

### Basic Counting and Sorting

**Intuition**

The simplest approach is to first count the frequency of each character in the string. Then, we can sort these characters based on their frequency in descending order and construct the resulting string based on this sorted order.

**Steps**

1. Count the frequency of each character using a hash map or array.
2. Store the characters in a list or vector and sort this list based on the frequency using a custom comparator.
3. Construct the final string by iterating through the sorted character list and appending each character its frequency times to the result.

**Time Complexity**: O(n log n), where n is the length of the string. Sorting the characters (n log n) is the major time-consuming step.

**Space Complexity**: O(n), space needed to store the frequency map and the result string.

```cpp
#include <iostream>
#include <unordered_map>
#include <vector>
#include <algorithm>

std::string frequencySort(std::string s) {
    // Step 1: Count frequency of each character
    std::unordered_map<char, int> freqMap;
    for (char c : s) {
        freqMap[c]++;
    }

    // Step 2: Store characters in a vector and sort them based on frequency
    std::vector<char> characters;
    for (auto& pair : freqMap) {
        characters.push_back(pair.first);
    }

    // Sort characters based on frequency
    std::sort(characters.begin(), characters.end(), [&](char a, char b) {
        return freqMap[a] > freqMap[b];
    });

    // Step 3: Construct result string
    std::string result;
    for (char c : characters) {
        result.append(freqMap[c], c);
    }

    return result;
}
```

---

### Priority Queue (Heap)

**Intuition**

By using a priority queue (max heap) to always extract the character with the highest frequency, we can optimize our approach slightly, as we avoid fully sorting the list based on frequency. The idea is to push all characters onto the heap and pop them to create the result in the desired order.

**Steps**

1. Use a hash map to count the frequency of each character.
2. Push each character with its frequency into a max heap (priority queue).
3. Continuously extract the maximum frequency character and append it to the result string.

**Time Complexity**: O(n log k), where n is the length of the string and k is the number of unique characters. Each heap insertion and removal takes log k time.

**Space Complexity**: O(n), mostly for the result and frequency map.

```cpp
#include <iostream>
#include <unordered_map>
#include <queue>
#include <string>

std::string frequencySort(std::string s) {
    // Step 1: Count frequency of each character
    std::unordered_map<char, int> freqMap;
    for (char c : s) {
        freqMap[c]++;
    }

    // Step 2: Use a max heap to sort characters by frequency
    std::priority_queue<std::pair<int, char>> maxHeap;
    for (auto& pair : freqMap) {
        maxHeap.push({pair.second, pair.first});
    }

    // Step 3: Construct result from heap
    std::string result;
    while (!maxHeap.empty()) {
        auto [freq, c] = maxHeap.top();
        maxHeap.pop();
        result.append(freq, c);
    }

    return result;
}
```

---

### Bucket Sort

**Intuition**

Bucket sort leverages the idea of grouping characters based on frequency. Using an array of vectors, we can store characters indexed by their frequency count. This takes advantage of O(1) access time for arrays, avoiding the overhead of sorting or heap operations.

**Steps**

1. Use a hash map to count the frequency of each character.
2. Use an array of vectors where the index represents frequency.
3. Iterate from the highest frequency downwards to construct the result.

**Time Complexity**: O(n), for counting and building the result.

**Space Complexity**: O(n), for storing frequencies and constructing the result.

```cpp
#include <iostream>
#include <unordered_map>
#include <vector>
#include <string>

std::string frequencySort(std::string s) {
    // Step 1: Count frequency of each character
    std::unordered_map<char, int> freqMap;
    for (char c : s) {
        freqMap[c]++;
    }

    // Determine the maximum frequency
    int maxFreq = 0;
    for (auto& pair : freqMap) {
        maxFreq = std::max(maxFreq, pair.second);
    }

    // Step 2: Create buckets where index represents frequency
    std::vector<std::vector<char>> buckets(maxFreq + 1);
    for (auto& pair : freqMap) {
        buckets[pair.second].push_back(pair.first);
    }

    // Step 3: Build the result from the buckets
    std::string result;
    for (int i = maxFreq; i > 0; --i) {
        for (char c : buckets[i]) {
            result.append(i, c);
        }
    }

    return result;
}
```

Each solution provides a different perspective and optimization for solving the "Sort Characters By Frequency" problem, ranging from simple sorting to more efficient heap and bucket sort techniques.

