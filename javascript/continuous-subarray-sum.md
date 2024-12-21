```markdown
# [Leetcode 523: Continuous Subarray Sum](https://leetcode.com/problems/continuous-subarray-sum/)

## Approaches:
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Using Remainder and HashMap](#approach-2-using-remainder-and-hashmap)

## Approach 1: Brute Force

### Intuition
The simplest way to solve this problem is to check every subarray of the given array `nums`. For each subarray, calculate its sum and check if the sum is a multiple of `k`. This approach is straightforward but can be inefficient for large arrays due to its time complexity.

### Implementation
```javascript
function checkSubarraySum(nums, k) {
    // Start with each subarray by fixing the starting point.
    for (let start = 0; start < nums.length; start++) {
        let sum = 0;
        // Expand the subarray by moving the end point.
        for (let end = start; end < nums.length; end++) {
            sum += nums[end];
            // Check if sum is a multiple of k, with at least two elements.
            if (end - start >= 1 && sum % k === 0) {
                return true;
            }
        }
    }
    return false;
}
```

### Time Complexity
- **O(n^2)**, where `n` is the length of the array. We check every subarray, resulting in a quadratic time complexity.

### Space Complexity
- **O(1)**, no extra space is used apart from a few variables.

---

## Approach 2: Using Remainder and HashMap

### Intuition
Instead of checking every subarray sum, we can use properties of modulo operations to optimize the approach. If two indices `i` and `j` (where `i < j`) have the same remainder when their cumulative sums are divided by `k`, then the subarray between these indices has a sum that is a multiple of `k`.

We maintain a running sum and use a hashmap to store the first occurrence of a particular remainder when divided by `k`. If the same remainder appears again at a different index, it means the subarray sum between these two indices is a multiple of `k`.

### Implementation
```javascript
function checkSubarraySum(nums, k) {
    // HashMap to store the first occurrence of each remainder
    let remainderMap = new Map();
    // Base case: sum 0 at index -1
    remainderMap.set(0, -1);
    let runningSum = 0;

    for (let i = 0; i < nums.length; i++) {
        runningSum += nums[i];
        let remainder = runningSum % k;
        // Handle negative remainder to ensure correct lookup
        if (remainder < 0) remainder += k;
        // Check if this remainder has been seen before
        if (remainderMap.has(remainder)) {
            // Verify if subarray has at least size 2
            if (i - remainderMap.get(remainder) > 1) {
                return true;
            }
        } else {
            // Store the first occurrence of this remainder
            remainderMap.set(remainder, i);
        }
    }
    return false;
}
```

### Time Complexity
- **O(n)**, where `n` is the length of the array. We only loop through the array once and perform constant-time operations for each element.

### Space Complexity
- **O(min(n, k))**, due to the storage of remainders in the hashmap. In the worst case, all remainders from 0 to `k-1` can be stored.

Each approach provides a progressively more optimal solution, with the second approach being suitable for larger input sizes due to its linear time complexity.
```

