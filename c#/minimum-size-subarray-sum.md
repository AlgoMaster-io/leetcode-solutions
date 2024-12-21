# [209. Minimum Size Subarray Sum](https://leetcode.com/problems/minimum-size-subarray-sum/)

## Approaches:
1. [Brute Force Approach](#brute-force-approach)
2. [Sliding Window Approach](#sliding-window-approach)

### Brute Force Approach

#### Intuition
The naive approach is to check all possible subarrays of the given array and find the smallest subarray whose sum is at least `s`. This requires checking every subarray, calculating their sums, and comparing with `s`.

#### Algorithm
1. Initialize `min_length` to a large number to keep track of the shortest subarray found.
2. Use two nested loops: the outer loop establishes a starting point of the subarray, and the inner loop finds possible endpoints for this subarray.
3. For each pair of starting and ending indices, calculate the sum of the subarray.
4. If the sum is greater than or equal to `s`, update `min_length`.
5. Continue until all subarrays are checked.

#### Code
```csharp
public int MinSubArrayLenBruteForce(int target, int[] nums) {
    int n = nums.Length;
    int minLength = int.MaxValue;
    
    for (int start = 0; start < n; start++) {
        int sum = 0;
        for (int end = start; end < n; end++) {
            sum += nums[end];
            // Check if the current subarray meets or exceeds the target
            if (sum >= target) {
                minLength = Math.Min(minLength, end - start + 1);
                break; // Since we're only interested in the first subarray meeting the condition
            }
        }
    }
    return minLength == int.MaxValue ? 0 : minLength;
}
```

#### Complexity Analysis
- **Time Complexity**: \(O(n^2)\), where \(n\) is the number of elements in the array. This is because every pair of indices is considered.
- **Space Complexity**: \(O(1)\), since no extra data structures are used.

### Sliding Window Approach

#### Intuition
The brute force method is inefficient because it recalculates the sum of overlapping subarrays multiple times. Instead, we can use a sliding window to dynamically adjust the subarray size. By expanding and contracting the window, we can efficiently find the minimal length.

#### Algorithm
1. Use two pointers, `left` and `right`, to represent the current subarray.
2. Expand `right` to increase the sum while it is less than `s`.
3. Once the sum is equal to or greater than `s`, try to minimize the subarray by moving `left`.
4. After moving `left` to the right, adjust sum and update `min_length` if necessary.
5. Continue adjusting the window until `right` has traversed the entire array.

#### Code
```csharp
public int MinSubArrayLenSlidingWindow(int target, int[] nums) {
    int n = nums.Length;
    int minLength = int.MaxValue;
    int sum = 0;
    int left = 0;
    
    for (int right = 0; right < n; right++) {
        sum += nums[right];
        // Contract the window as small as possible while the sum is >= target
        while (sum >= target) {
            minLength = Math.Min(minLength, right - left + 1);
            sum -= nums[left++];
        }
    }
    
    return minLength == int.MaxValue ? 0 : minLength;
}
```

#### Complexity Analysis
- **Time Complexity**: \(O(n)\) since each element is processed at most twice (once added to the sum, once removed).
- **Space Complexity**: \(O(1)\), as no additional space is used.

This problem showcases the power and efficiency of the sliding window technique, significantly optimizing what would otherwise be a less feasible solution using brute force.

