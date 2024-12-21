# [Leetcode: Max Consecutive Ones III](https://leetcode.com/problems/max-consecutive-ones-iii/)

## Approaches
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Sliding Window (Optimal)](#approach-2-sliding-window-optimal)

### Approach 1: Brute Force

#### Intuition:
The brute force approach involves checking every possible subarray to determine the maximum number of consecutive ones you can achieve by flipping at most `k` zeros. This requires iterating over each subarray and counting the zeros in it.

#### Steps:
1. Iterate through each possible starting point for the subarray.
2. From each starting point, extend the subarray and count the number of zeros encountered.
3. If the number of zeros exceeds `k`, break out of the loop.
4. Keep track of the maximum length of such a subarray.

#### Code:
```java
public int longestOnes(int[] nums, int k) {
    int maxLength = 0;
    for (int i = 0; i < nums.length; i++) {
        int zerosCount = 0;
        for (int j = i; j < nums.length; j++) {
            if (nums[j] == 0) zerosCount++;
            if (zerosCount > k) break;
            maxLength = Math.max(maxLength, j - i + 1);
        }
    }
    return maxLength;
}
```

#### Complexity:
- **Time Complexity:** \(O(n^2)\) - We examine each subarray starting from every index.
- **Space Complexity:** \(O(1)\) - We use constant space for counters and storage.

### Approach 2: Sliding Window (Optimal)

#### Intuition:
The sliding window technique efficiently finds the longest subarray with at most `k` zeros. The idea is to use two pointers to form a window and adjust it to keep track of the valid subarray as:
1. Extend the right pointer to find a valid subarray with at most `k` zeros.
2. If zeros exceed `k`, shift the left pointer to make the subarray valid again by reducing the zeros count.

#### Steps:
1. Initialize two pointers `left` and `right` at the start of the array.
2. Increment the `right` pointer to expand the window.
3. Whenever a zero is encountered, increment the zero count.
4. If the number of zeros exceeds `k`, increment the `left` pointer until the zeros count within the window is not more than `k`.
5. Keep track of the maximum window size encountered.

#### Code:
```java
public int longestOnes(int[] nums, int k) {
    int left = 0, right = 0, zerosCount = 0, maxLength = 0;
    while (right < nums.length) {
        // Extend the window with the `right` pointer
        if (nums[right] == 0) zerosCount++;

        // If zeros in the window exceed k, shrink from the left
        while (zerosCount > k) {
            if (nums[left] == 0) zerosCount--;
            left++;
        }
        // Calculate the maximum length of valid subarray so far
        maxLength = Math.max(maxLength, right - left + 1);
        right++;
    }
    return maxLength;
}
```

#### Complexity:
- **Time Complexity:** \(O(n)\) - Each element is processed at most twice (once by `right` and once by `left`).
- **Space Complexity:** \(O(1)\) - We use constant space for pointers and counters.

This optimal approach significantly reduces the time complexity by leveraging the sliding window technique to find the maximum length dynamically.

