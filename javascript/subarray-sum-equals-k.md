# [560. Subarray Sum Equals K](https://leetcode.com/problems/subarray-sum-equals-k/)

## Approaches

1. [Brute Force](#approach-1-brute-force)
2. [Using HashMap (Prefix Sum)](#approach-2-using-hashmap-prefix-sum)

---

## Approach 1: Brute Force

This is the simplest method, where we check every possible subarray to see if the sum matches the desired value `k`. We use two nested loops to generate all possible subarrays.

### Intuition

The idea is straightforward: iterate through all subarrays, calculate their sums, and check if the sum equals `k`. Although simple, this method is not efficient for large arrays because it's quadratic in nature.

### Algorithm

1. Initialize a counter `count` to zero. This will store the number of subarrays with sum equal to `k`.
2. Use a loop `i` from `0` to `n-1` and a nested loop `j` from `i` to `n-1` to consider every subarray starting with index `i`.
3. For every subarray `[i...j]`, calculate its sum.
4. If the sum equals `k`, increment the counter.
5. Return the counter.

### Code

```javascript
function subarraySum(nums, k) {
    let count = 0;
    let n = nums.length;

    // Iterate over all subarrays
    for (let i = 0; i < n; i++) {
        let currentSum = 0;

        // Calculate the sum from index i to j
        for (let j = i; j < n; j++) {
            currentSum += nums[j];

            // If current subarray sum is equal to k, increase the count
            if (currentSum === k) {
                count++;
            }
        }
    }

    return count;
}
```

### Complexity Analysis

- **Time Complexity**: O(n^2), where `n` is the number of elements in the array. We explore all possible subarrays.
- **Space Complexity**: O(1), no extra space is used except for a few variables.

---

## Approach 2: Using HashMap (Prefix Sum)

This method improves efficiency by using a hash map to store the prefix sums and their frequencies. The key idea is to use the prefix sum to determine if there's a subarray with sum `k`.

### Intuition

In order to efficiently find subarrays summing up to `k`, we use the prefix sum technique along with a hash map. The prefix sum allows us to compute the sum of any subarray in constant time. By saving previously seen prefix sums in a hash map, we can swiftly check if there exists a prefix sum such that the difference between the current prefix sum and the required sum `k` exists.

### Algorithm

1. Initialize a hash map `prefixSumCount` to store frequency of prefix sums and set it to have a key `0` with value `1` (to handle the case where the subarray itself starts from index `0`).
2. Initialize `currentSum` to `0` and `count` to `0`.
3. Traverse through the array:
   - Add the current element to `currentSum`.
   - Compute `neededSum` as `currentSum - k`.
   - If `neededSum` exists in `prefixSumCount`, it indicates that there is a subarray ending at the current index which sums to `k`. Thus, add its frequency to `count`.
   - Update `prefixSumCount` by adding `currentSum` to it.
4. Return the `count`.

### Code

```javascript
function subarraySum(nums, k) {
    let count = 0;
    let currentSum = 0;
    const prefixSumCount = new Map();
    prefixSumCount.set(0, 1);

    for (let num of nums) {
        // Update the current sum
        currentSum += num;

        // Calculate the needed sum that when subtracted from the current sum gives k
        let neededSum = currentSum - k;

        // If the needed sum exists in the map, increase the count by the frequency of the needed sum
        if (prefixSumCount.has(neededSum)) {
            count += prefixSumCount.get(neededSum);
        }

        // Update the frequency of the current sum in the map
        prefixSumCount.set(currentSum, (prefixSumCount.get(currentSum) || 0) + 1);
    }

    return count;
}
```

### Complexity Analysis

- **Time Complexity**: O(n), where `n` is the number of elements in the array. We traverse the array once.
- **Space Complexity**: O(n), for storing the prefix sums in the hash map. In the worst case, all prefix sums are unique.

This hashmap approach is optimal for this problem, balancing time complexity and space complexity efficiently.

