# [Leetcode 767: Reorganize String](https://leetcode.com/problems/reorganize-string/)

## Approaches:
- [Approach 1: Greedy Approach with Max Heap](#approach-1-greedy-approach-with-max-heap)
- [Approach 2: Efficient Two-pointer Technique](#approach-2-efficient-two-pointer-technique)

---

## Approach 1: Greedy Approach with Max Heap

### Intuition
The main idea is to always place the most frequent character available that can legally be placed next. By using a max heap, we can always fetch the character with the highest frequency and place it in the result. If the most frequent character is the same as the last character placed in the result, it should be skipped until another character is placed.

1. **Count Frequencies**: Calculate the frequency of each character.
2. **Max Heap**: Use a max heap to store characters based on frequency, so that the character with the highest frequency can be fetched easily.
3. **Reorganize**: Start constructing the string by picking the character with the maximum frequency from the heap, making sure not to place the same character consecutively.
4. **Edge Cases**: If the frequency of any character is greater than `(n + 1) / 2`, where `n` is the length of the string, it is impossible to reorganize the string.

### Implementation
```cpp
#include <iostream>
#include <string>
#include <unordered_map>
#include <vector>
#include <queue>

using namespace std;

string reorganizeString(string s) {
    unordered_map<char, int> frequency;
    int n = s.size();

    // Count the frequency of each character
    for (char c : s) {
        frequency[c]++;
    }

    // Define a max heap based on frequency
    priority_queue<pair<int, char>> maxHeap;
    for (auto& [char, freq] : frequency) {
        if (freq > (n + 1) / 2) return ""; // return empty if impossible
        maxHeap.push({freq, char});
    }

    // Result string
    string result = "";
    
    // While the heap is not empty
    while (maxHeap.size() > 1) {
        // Get the two most frequent characters
        auto [freq1, char1] = maxHeap.top(); maxHeap.pop();
        auto [freq2, char2] = maxHeap.top(); maxHeap.pop();

        // Append them to result
        result += char1;
        result += char2;

        // Decrease their frequency and re-add if they are still greater than zero
        if (--freq1 > 0) {
            maxHeap.push({freq1, char1});
        }
        if (--freq2 > 0) {
            maxHeap.push({freq2, char2});
        }
    }

    // If there's an odd character out, append it
    if (!maxHeap.empty()) {
        if (maxHeap.top().first > 1) return "";
        result += maxHeap.top().second;
    }

    return result;
}
```
### Time Complexity
- **O(n log k)**, where `n` is the length of the string and `k` is the number of unique characters. The heap operations (push/pop) take `O(log k)` time.
  
### Space Complexity
- **O(k)**, since we use a priority queue to store the characters.

---

## Approach 2: Efficient Two-pointer Technique

### Intuition
This approach uses two pointers to effectively manage character positioning. Fill the string in two passes: one for the even indices and another for the odd indices, ensuring that no two adjacent characters are the same.

1. **Count Frequencies**: Store frequencies and sort the characters based on frequencies.
2. **Place In Even Indices**: Traverse the character array, placing characters starting from index 0.
3. **Place In Odd Indices**: Once the end is reached, fill out remaining in odd indices.
4. **Reorganization Check**: The goal is to ensure no two adjacent characters are the same by placing the most frequent characters first in alternate indices.

### Implementation
```cpp
#include <iostream>
#include <string>
#include <unordered_map>
#include <vector>
#include <algorithm>

using namespace std;

string reorganizeStringTwoPointer(string s) {
    unordered_map<char, int> frequency;
    int n = s.size();

    // Count the frequency of each character
    for (char c : s) {
        frequency[c]++;
    }

    // Sort characters by frequency
    vector<pair<int, char>> freq_chars;
    for (auto& elem : frequency) {
        freq_chars.push_back({elem.second, elem.first});
    }
    sort(freq_chars.rbegin(), freq_chars.rend());

    // Check if solution is possible
    if (freq_chars[0].first > (n + 1) / 2) return "";

    string result(n, ' ');

    int index = 0;
    for (auto& [freq, char_val] : freq_chars) {
        for (int count = 0; count < freq; ++count) {
            // Fill even indices first
            if (index >= n) {
                index = 1; // switch to odd index if even indices are filled
            }
            result[index] = char_val;
            index += 2;
        }
    }

    return result;
}
```
### Time Complexity
- **O(n)**, including counting frequencies and filling the result array.

### Space Complexity
- **O(k)**, where `k` is the number of unique characters for storing frequency mapping and sorted results.

---

Both approaches provide efficient solutions to ensure the conditions for re-organization are met, intelligently managing character placement through greedy strategies.

