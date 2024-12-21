# [Leetcode 2348: Number of Zero-Filled Subarrays](https://leetcode.com/problems/number-of-zero-filled-subarrays/)

## Approaches:
- [Brute Force](#brute-force)
- [Optimized Counting](#optimized-counting)

### Brute Force

#### Intuition:
The naive solution can be to iterate through each subarray and count the number of zero-filled subarrays. While this approach is straightforward, it is not efficient, as it will involve checking every possible subarray in the array.

#### Approach:
1. Initialize a count to store the total number of zero-filled subarrays.
2. Iterate over each starting index i of the array.
3. For each starting index, initiate a nested loop starting from index i.
4. Keep a flag to check if the current subarray is a zero-filled subarray.
5. If a non-zero element is found, break the inner loop.
6. If the subarray from index i to j is zero-filled, increment the count.

#### Code:
```cpp
int zeroFilledSubarray(vector<int>& nums) {
    int n = nums.size();
    int count = 0;
    
    // Loop through each element and consider it as the start of a subarray
    for (int i = 0; i < n; ++i) {
        // This will store if the current subarray is zero-filled
        bool isZeroFilled = true;
        
        // Check from the current start of the subarray to the end
        for (int j = i; j < n; ++j) {
            // If a non-zero is encountered, break the loop
            if (nums[j] != 0) {
                break;
            }
            // Increment count for each zero-filled subarray found
            ++count;
        }
    }
    return count;
}
```

#### Complexity:
- **Time Complexity**: O(n^2), where n is the length of the array. For each element, we might end up checking all subarrays starting from that element.
- **Space Complexity**: O(1), as no additional space is used.

### Optimized Counting

#### Intuition:
The problem can be approached more optimally by considering the fact that a subarray consisting of zeros continues to contribute new zero-filled subarrays as new zeros are encountered. Instead of checking each subarray explicitly, count the length of contiguous zeros, and calculate the number of subarrays that can be formed from these zeros.

#### Approach:
1. Initialize a count variable `zeroCount` to count the length of consecutive zeros.
2. Iterate through the array.
3. If a zero is encountered, increment `zeroCount`. 
4. For each zero, add `zeroCount` to the total number of subarrays, as each zero extends prior subarrays and forms new subarrays.
5. If a non-zero is encountered, reset `zeroCount` to zero.

#### Code:
```cpp
int zeroFilledSubarray(vector<int>& nums) {
    int totalCount = 0;   // Total zero-filled subarrays
    int zeroCount = 0;    // Count of consecutive zeros

    for (int num : nums) {
        if (num == 0) {
            ++zeroCount;  // Increase zero streak
            totalCount += zeroCount;  // Add all possible subarrays formed by this streak
        } else {
            zeroCount = 0;  // Reset streak on encountering a non-zero
        }
    }
    return totalCount;
}
```

#### Complexity:
- **Time Complexity**: O(n), where n is the length of the array, as we traverse the array only once.
- **Space Complexity**: O(1), as we use only constant extra space.

