# [1696. Jump Game VI](https://leetcode.com/problems/jump-game-vi/)

## Approach 1: Dynamic Programming with Sliding Window (Deque)

### Solution
cpp
```cpp
// Time Complexity: O(n)
// Space Complexity: O(n)
#include <deque>
#include <vector>
#include <algorithm>

class Solution {
public:
    int maxResult(std::vector<int>& nums, int k) {
        std::deque<int> deque; // Stores indices of potential max scores
        deque.push_back(0);
        std::vector<int> dp(nums.size());
        dp[0] = nums[0];

        for (int i = 1; i < nums.size(); ++i) {
            // Remove indices that are out of the current window
            if (!deque.empty() && deque.front() < i - k) {
                deque.pop_front();
            }

            // Update dp[i] using the max value in the deque
            dp[i] = nums[i] + dp[deque.front()];

            // Maintain the deque to keep indices in decreasing order of their dp values
            while (!deque.empty() && dp[deque.back()] <= dp[i]) {
                deque.pop_back();
            }

            deque.push_back(i);
        }

        return dp.back();
    }
};
```

## Approach 2: Priority Queue (Heap-Based)

### Solution
cpp
```cpp
// Time Complexity: O(n log k)
// Space Complexity: O(k)
#include <queue>
#include <vector>

class Solution {
public:
    int maxResult(std::vector<int>& nums, int k) {
        std::priority_queue<std::pair<int, int>> maxHeap; // Max-heap
        maxHeap.emplace(nums[0], 0); // Store {score, index}
        int score = nums[0];

        for (int i = 1; i < nums.size(); ++i) {
            // Remove elements outside the current window
            while (maxHeap.top().second < i - k) {
                maxHeap.pop();
            }

            // The current score is the sum of the max score in the heap and nums[i]
            score = nums[i] + maxHeap.top().first;
            maxHeap.emplace(score, i);
        }

        return score;
    }
};
```

