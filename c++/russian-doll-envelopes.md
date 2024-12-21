# [Leetcode 354: Russian Doll Envelopes](https://leetcode.com/problems/russian-doll-envelopes/)

## Navigation
- [Approach 1: Dynamic Programming with Sorting](#Approach-1)
- [Approach 2: Binary Search with Sorting and Patience Sorting (Optimal)](#Approach-2)

## Approach 1: Dynamic Programming with Sorting
The problem of finding the maximum number of envelopes that can be nested inside each other is similar to finding the longest increasing subsequence in a sequence of numbers. Here, each envelope is a pair, and we need to find a sequence of pairs where each pair is strictly larger than the previous one in both dimensions.

### Intuition:
1. **Sorting**: First, we sort the envelopes by width in ascending order, and by height in descending order if the widths are the same. This sorting ensures that we only need to focus on increasing sequences of heights, as increasing widths are already assured by the initial sort.
2. **Dynamic Programming**: We then apply dynamic programming to find the longest increasing subsequence of heights in the sorted list of envelopes.

### Solution:

```cpp
#include <vector>
#include <algorithm>

using namespace std;

int maxEnvelopes(vector<vector<int>>& envelopes) {
    if(envelopes.empty()) return 0;

    // Sort the envelopes. If width is the same, sort by height in descending order
    sort(envelopes.begin(), envelopes.end(), [](const vector<int>& a, const vector<int>& b){
        return a[0] == b[0] ? a[1] > b[1] : a[0] < b[0];
    });
    
    // Extract the 'heights' in sorted order
    vector<int> heights;
    for(const auto& envelope : envelopes)
        heights.push_back(envelope[1]);

    // Find the length of the longest increasing subsequence in 'heights'
    if(heights.empty()) return 0;

    vector<int> dp(heights.size(), 1);
    int max_len = 1;
    for(size_t i = 1; i < heights.size(); ++i) {
        for(size_t j = 0; j < i; ++j) {
            if(heights[i] > heights[j]) {
                dp[i] = max(dp[i], dp[j] + 1);
            }
        }
        max_len = max(max_len, dp[i]);
    }
    return max_len;
}
```

### Time Complexity:
- **O(n^2)** due to the double loop to compute the LIS using dynamic programming.

### Space Complexity:
- **O(n)**, where n is the number of envelopes, for storing the dynamic programming table.

---

## Approach 2: Binary Search with Sorting and Patience Sorting (Optimal)

For an optimal approach, we utilize a method inspired by patience sorting, which maintains a list where the length of the list represents the length of the longest increasing subsequence found.

### Intuition:
1. **Sorting**: Similar to the first approach, sort the envelopes by width and height.
2. **Patience Sorting with Binary Search**: Use a vector to maintain the current increasing subsequence. For each envelope's height, use binary search to find its position in the current longest sequence; if it extends the sequence, append it. If it can replace an element, replace the largest number to maintain the smallest possible ending number for potential further increasing sequences.

### Solution:

```cpp
#include <vector>
#include <algorithm>

using namespace std;

int maxEnvelopes(vector<vector<int>>& envelopes) {
    if(envelopes.empty()) return 0;

    // Sort the envelopes. If width is the same, sort by height in descending order
    sort(envelopes.begin(), envelopes.end(), [](const vector<int>& a, const vector<int>& b){
        return a[0] == b[0] ? a[1] > b[1] : a[0] < b[0];
    });
    
    // Extract the 'heights' in sorted order
    vector<int> heights;
    for(const auto& envelope : envelopes)
        heights.push_back(envelope[1]);

    // Patience sort + longest increasing subsequence using binary search
    vector<int> lis;
    for(int height : heights) {
        auto it = lower_bound(lis.begin(), lis.end(), height);
        if(it == lis.end()) {
            lis.push_back(height);
        } else {
            *it = height;
        }
    }
    return lis.size();
}
```

### Time Complexity:
- **O(n log n)** due to the binary search used in determining where to place each height.

### Space Complexity:
- **O(n)**, which is space used by the longest increasing subsequence.

