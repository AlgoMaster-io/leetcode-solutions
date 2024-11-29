# [2461. Maximum Sum of Distinct Subarrays With Length K](https://leetcode.com/problems/maximum-sum-of-distinct-subarrays-with-length-k/)

## Approach 1: Brute Force (Basic)

### Solution
```cpp
// Time Complexity: O(n * k)
// Space Complexity: O(k)
#include <vector>
#include <unordered_set>
#include <algorithm>

class Solution {
public:
    long long maximumSubarraySum(std::vector<int>& nums, int k) {
        long long maxSum = 0;

        // Iterate over every subarray of size k
        for (int i = 0; i <= nums.size() - k; i++) {
            std::unordered_set<int> uniqueElements;
            long long sum = 0;
            bool isValid = true;

            for (int j = i; j < i + k; j++) {
                if (uniqueElements.find(nums[j]) != uniqueElements.end()) {
                    isValid = false; // Subarray contains duplicate
                    break;
                }
                uniqueElements.insert(nums[j]);
                sum += nums[j];
            }

            if (isValid) {
                maxSum = std::max(maxSum, sum);
            }
        }

        return maxSum;
    }
};
```

## Approach 2: Sliding Window with HashMap (Optimal)

### Solution
```cpp
// Time Complexity: O(n)
// Space Complexity: O(k)
#include <vector>
#include <unordered_map>
#include <algorithm>

class Solution {
public:
    long long maximumSubarraySum(std::vector<int>& nums, int k) {
        long long maxSum = 0, currentSum = 0;
        std::unordered_map<int, int> frequencyMap;
        int left = 0;

        // Sliding window
        for (int right = 0; right < nums.size(); right++) {
            currentSum += nums[right];
            frequencyMap[nums[right]]++;

            // If the window size exceeds k, shrink it
            if (right - left + 1 > k) {
                currentSum -= nums[left];
                frequencyMap[nums[left]]--;
                if (frequencyMap[nums[left]] == 0) {
                    frequencyMap.erase(nums[left]);
                }
                left++;
            }

            // Check if the current window is valid (distinct elements)
            if (right - left + 1 == k && frequencyMap.size() == k) {
                maxSum = std::max(maxSum, currentSum);
            }
        }

        return maxSum;
    }
};
```

