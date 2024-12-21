# [Leetcode 209: Minimum Size Subarray Sum](https://leetcode.com/problems/minimum-size-subarray-sum/)

## Approaches
1. [Brute Force](#brute-force)
2. [Sliding Window](#sliding-window)

### Brute Force

#### Intuition
The brute force approach involves evaluating all possible subarrays for a given array to determine if they meet the minimal size requirement while also having a sum that is greater than or equal to the target sum. This is done by considering each possible starting and ending point and checking if the subarray formed by these points satisfies the target condition.

#### Approach
1. Iterate over every starting index of the subarray.
2. For each starting index, iterate over every possible ending index to complete the subarray.
3. Calculate the sum of the elements in the current subarray.
4. Check if the sum is greater than or equal to the target sum. If it is, compute the length of the subarray and update the minimum length found so far.
5. Return the minimum length found or 0 if no valid subarray is found.

#### Code
```cpp
int minSubArrayLen(int target, vector<int>& nums) {
    int n = nums.size();
    int minLength = INT_MAX;

    // Iterate over each possible start index
    for (int start = 0; start < n; ++start) {
        int sum = 0;
        // Iterate and find all end indices
        for (int end = start; end < n; ++end) {
            sum += nums[end];
            // Check if the current subarray meets the target
            if (sum >= target) {
                minLength = min(minLength, end - start + 1);
                break;
            }
        }
    }
    return minLength == INT_MAX ? 0 : minLength;
}
```

#### Complexity
- **Time Complexity**: O(n^2) - We consider each subarray by using a nested loop.
- **Space Complexity**: O(1) - No additional space required.

### Sliding Window

#### Intuition
A more efficient approach uses the sliding window technique, which allows us to find the minimum-length subarray sum by dynamically adjusting the window size as we move along the array. The idea is to maintain a window that encompasses the smallest subarray that meets the target condition by expanding and contracting the window efficiently.

#### Approach
1. Use two pointers (or indices): `start` and `end` to signify the current window's limits.
2. Expand the window by moving the `end` pointer to the right and add the corresponding element to the current sum.
3. Once the sum is equal to or greater than the target, contract the window by moving the `start` pointer to the right to possibly find a smaller valid window.
4. Update the minimum window size each time a valid window is found.
5. Continue adjusting the window size until the `end` has traversed the entire array.

#### Code
```cpp
int minSubArrayLen(int target, vector<int>& nums) {
    int n = nums.size();
    int minLength = INT_MAX;
    int sum = 0;
    int start = 0;

    // Iterate with an expanding end index
    for (int end = 0; end < n; ++end) {
        sum += nums[end];
        
        // Reduce the window size while condition is satisfied
        while (sum >= target) {
            minLength = min(minLength, end - start + 1);
            sum -= nums[start++];
        }
    }
    
    return minLength == INT_MAX ? 0 : minLength;
}
```

#### Complexity
- **Time Complexity**: O(n) - Each element is accessed at most twice.
- **Space Complexity**: O(1) - Only a constant amount of extra space is used.

### Recommended Approach
The sliding window technique is the most efficient and recommended approach due to its linear time complexity compared to the quadratic time complexity of the brute force method.

