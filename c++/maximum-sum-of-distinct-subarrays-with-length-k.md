# [Leetcode 2461: Maximum Sum of Distinct Subarrays With Length K](https://leetcode.com/problems/maximum-sum-of-distinct-subarrays-with-length-k/)

## Approaches:
1. [Sliding Window with Set](#approach-1-sliding-window-with-set)
2. [Sliding Window with Frequency Array](#approach-2-sliding-window-with-frequency-array)

---

## Approach 1: Sliding Window with Set

### Intuition:
The problem asks us to find the maximum sum of a subarray with distinct elements and of size `k`. This kind of windowed subarray problem can naturally be approached with a sliding window algorithm. We use a set to ensure the elements within the current window are distinct.

The basic idea is to maintain a sliding window of size `k` and keep track of elements in the window using a set. If an element is duplicated within the window (detected by checking the set), we shrink the window until all elements are distinct. We keep track of the maximum sum achieved with distinct elements.

### Detailed Steps:
1. Use two pointers to represent the sliding window - `start` and `end`.
2. Iterate `end` over the array:
    - Add element at `end` to the current window sum.
    - If the element at `end` is already in the set, increment the `start` pointer until the element can be added uniquely.
    - If the size of the window is exactly `k`, update the maximum sum if the current sum is higher.
3. Return the maximum sum found.

```cpp
#include <iostream>
#include <vector>
#include <unordered_set>
using namespace std;

int maximumSubarraySum(vector<int>& nums, int k) {
    unordered_set<int> window;   // Set to store unique elements in the window.
    int start = 0, sum = 0, maxSum = 0;
    
    for (int end = 0; end < nums.size(); ++end) {
        // Try to add the current element to the window.
        while (window.find(nums[end]) != window.end()) {
            // If element is duplicate, slide the window from start
            sum -= nums[start];            // Remove the start element from sum
            window.erase(nums[start]);  // Remove the element from the set
            start++;                   // Slide the window start
        }
        // Add the current element to sum and set, increasing the window size.
        sum += nums[end];
        window.insert(nums[end]);

        // If window size = k, check if we found a larger sum
        if (end - start + 1 == k) {
            maxSum = max(maxSum, sum);
            // Slide window to explore further subarrays of length k
            sum -= nums[start];
            window.erase(nums[start]);
            start++;
        }
    }
    return maxSum;
}

int main() {
    vector<int> nums = {1, 2, 1, 3, 5, 2};
    int k = 3;
    cout << "Maximum sum of distinct subarrays with length " << k << " is: " << maximumSubarraySum(nums, k) << endl;
    return 0;
}
```
- **Time Complexity:** O(n) - Each element is processed at most twice.
- **Space Complexity:** O(k) - The set holds at most `k` distinct elements.

---

## Approach 2: Sliding Window with Frequency Array

### Intuition:
To optimize space, we can replace the set with a frequency array. This allows us to keep track of the count of each element within the current window, thus maintaining distinct elements. If any count exceeds 1, we know there's a duplicate, and we adjust the window accordingly.

### Detailed Steps:
1. Maintain a frequency array to count occurrences of elements in the current window.
2. Use two pointers to represent window bounds.
3. Iterate `end` over the array:
    - Increase the count of the element at `end`.
    - If `end - start + 1` exceeds `k`, we slide the window from `start`.
    - If the frequency of any element becomes > 1, adjust the window until you remove duplicates.
    - Check if the current window size is `k` and update maximum sum if needed.
4. Return the maximum sum found.

```cpp
#include <iostream>
#include <vector>
using namespace std;

int maximumSubarraySum(vector<int>& nums, int k) {
    vector<int> freq(100001, 0);  // Frequency array to count element occurrences.
    int start = 0, sum = 0, maxSum = 0;
    
    for (int end = 0; end < nums.size(); ++end) {
        freq[nums[end]]++;             // Include current element in frequency count.
        sum += nums[end];              // Add current element to the running sum.

        // Slide window if there are any duplicates
        while (freq[nums[end]] > 1) {
            freq[nums[start]]--;       // Reduce frequency of start element.
            sum -= nums[start];        // Remove start element from the sum.
            start++;                   // Move start of the window forward.
        }

        // Evaluate the result if window size is exactly k
        if (end - start + 1 == k) {
            maxSum = max(maxSum, sum);
            // Ready the window for a new subarray by moving start
            freq[nums[start]]--;
            sum -= nums[start];
            start++;
        }
    }
    return maxSum;
}

int main() {
    vector<int> nums = {1, 2, 1, 3, 5, 2};
    int k = 3;
    cout << "Maximum sum of distinct subarrays with length " << k << " is: " << maximumSubarraySum(nums, k) << endl;
    return 0;
}
```
- **Time Complexity:** O(n) - Each element is processed at most twice.
- **Space Complexity:** O(1) - The frequency array size is constant regarding input size since elements have a fixed limit.

Both approaches efficiently solve the problem of finding the maximum sum of distinct subarrays with the given length `k`, giving a strong understanding of sliding window techniques in distinct element scenarios.

