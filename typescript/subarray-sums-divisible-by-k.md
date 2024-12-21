# [974. Subarray Sums Divisible by K](https://leetcode.com/problems/subarray-sums-divisible-by-k/)

## Approaches:
- [Brute Force Approach](#brute-force-approach)
- [Prefix Sums with Hashmap](#prefix-sums-with-hashmap)

### Brute Force Approach

#### Intuition:
The brute force solution checks every possible subarray of the array and calculates its sum to determine if that sum is divisible by `k`. This approach involves iterating through all the possible start and end indices of subarrays.

#### Steps:
1. Iterate with a starting pointer `i` over the array from `0` to `n-1`.
2. For each `i`, iterate with an ending pointer `j` from `i` to `n-1`.
3. Calculate the sum of the elements from index `i` to `j`.
4. Check if the sum is divisible by `k`. If yes, increase the count.
5. Continue for all possible subarrays.

#### Code:
```typescript
function subarraysDivByK(nums: number[], k: number): number {
    let count = 0;

    for (let i = 0; i < nums.length; i++) {
        let sum = 0;
        for (let j = i; j < nums.length; j++) {
            sum += nums[j]; // Calculate the sum of the current subarray
            if (sum % k === 0) { // Check for divisibility by k
                count++; // Increment count when divisible
            }
        }
    }

    return count;
}
```

#### Time Complexity:
- O(n^2), where `n` is the length of the array.
- We have two nested loops, each iterating over the array.

#### Space Complexity:
- O(1). No extra space is used.

### Prefix Sums with Hashmap

#### Intuition:
Instead of recalculating the sum of every subarray, we can use prefix sums to store remainders. We calculate the prefix sum for each element and use a hashmap to store occurrences of each remainder when divided by `k`. If the same remainder has appeared before, it indicates that the subarray between these indices is divisible by `k`.

#### Steps:
1. Initialize a `remainderMap` with a default count for a remainder of 0 to handle the case where a prefix itself is divisible.
2. Maintain a `cumulativeSum` that calculates the prefix sum iteratively.
3. For each element:
   - Update the cumulative sum.
   - Calculate `remainder = cumulativeSum % k`. Adjust the remainder to be positive.
   - If `remainder` has been seen before in `remainderMap`, add its frequency to the `count` since it means there are several subarrays ending at the current index that are divisible by `k`.
   - Update the `remainderMap` with the new count for the current `remainder`.

#### Code:
```typescript
function subarraysDivByK(nums: number[], k: number): number {
    const remainderMap = new Map<number, number>();
    remainderMap.set(0, 1); // Initialize with 0 remainder seen 1 time
    let cumulativeSum = 0;
    let count = 0;

    for (const num of nums) {
        cumulativeSum += num; // Update cumulative sum
        let remainder = cumulativeSum % k; // Calculate remainder
        
        if (remainder < 0) remainder += k; // Adjust for negative remainders
        
        if (remainderMap.has(remainder)) {
            count += remainderMap.get(remainder)!; // Add the frequency to count
        }
        
        remainderMap.set(remainder, (remainderMap.get(remainder) || 0) + 1); // Update the remainder frequency
    }

    return count;
}
```

#### Time Complexity:
- O(n), where `n` is the length of the array.
- Single pass through the array with constant-time hash map operations.

#### Space Complexity:
- O(min(n, k)), the space used by the hashmap to maintain remainders, where the maximum number of unique remainders is limited by `k`.

