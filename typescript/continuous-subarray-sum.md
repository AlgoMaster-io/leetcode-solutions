## [Leetcode Problem 523: Continuous Subarray Sum](https://leetcode.com/problems/continuous-subarray-sum/)

### Approaches:
1. [Brute Force Approach](#1-brute-force-approach)
2. [Optimized Using HashMap](#2-optimized-using-hashmap)

### 1. Brute Force Approach

#### Intuition
The brute force solution for this problem involves iterating over all possible subarrays of the input list and checking if any subarray has a sum that is a multiple of `k`. This method will check all possible combinations of subarray start and end indices.

#### Steps:
- Iterate over all possible starting points of subarrays.
- For each starting point, calculate the sum of elements up to every other element that comes after it.
- Check if the sum is a multiple of `k`.

```typescript
function checkSubarraySum(nums: number[], k: number): boolean {
    // Loop over all possible starting indices of subarrays
    for (let start = 0; start < nums.length; start++) {
        let sum = 0;
        // Sum elements from start to the current end index
        for (let end = start; end < nums.length; end++) {
            sum += nums[end];
            // Check if the sum is non-zero and a multiple of k
            if (end > start && sum % k === 0) {
                return true;
            }
        }
    }
    return false;
}
```

- **Time Complexity**: O(n^2)
- **Space Complexity**: O(1)

### 2. Optimized Using HashMap

#### Intuition:
The optimized approach leverages the properties of modular arithmetic and uses a hashmap to store the cumulative sum mods seen so far. By storing these, we can check if we have encountered a previous sum with the same mod value as the current sum, which implies the presence of a subarray with zero sum difference modulo `k`.

#### Steps:
- Use a HashMap to store the remainders of the cumulative sum when divided by `k`.
- Iterate through the array while maintaining a cumulative sum.
- If the current cumulative sum's remainder has been seen before, it indicates a subarray whose sum is a multiple of `k`.
- To handle the edge case, initialize the map with (0, -1) to cover the scenario where a valid subarray starts from index zero.

```typescript
function checkSubarraySum(nums: number[], k: number): boolean {
    const remainderMap: Map<number, number> = new Map<number, number>();
    remainderMap.set(0, -1);  // Initialize with 0 to cater for the subarrays starting at 0
    let sum = 0;

    for (let i = 0; i < nums.length; i++) {
        sum += nums[i];
        const remainder = sum % k;
        
        // If remainder is negative, convert it to positive equivalent
        const normalizedRemainder = remainder < 0 ? remainder + k : remainder;

        // Check if this remainder was seen before
        if (remainderMap.has(normalizedRemainder)) {
            if (i - remainderMap.get(normalizedRemainder)! > 1) {
                return true;
            }
        } else {
            // Store this remainder with its index in the map
            remainderMap.set(normalizedRemainder, i);
        }
    }
    return false;
}
```

- **Time Complexity**: O(n)
- **Space Complexity**: O(min(n, k)) since we store remainders of the cumulative sums up to k times at most.

Through these approaches, we go from a straightforward but inefficient method to a more refined and efficient algorithm using **hash maps**, significantly enhancing performance for larger inputs.

