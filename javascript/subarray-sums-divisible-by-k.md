# [974. Subarray Sums Divisible by K](https://leetcode.com/problems/subarray-sums-divisible-by-k/)

## Table of Contents
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Efficient Prefix Sum with Hash Map](#approach-2-efficient-prefix-sum-with-hash-map)

## Approach 1: Brute Force

### Intuition
The most straightforward way to solve this problem is to check every possible subarray and verify if the sum of elements in the subarray is divisible by `K`. 

### Approach
1. Iterate over all starting points of subarrays.
2. For each starting point, iterate over all possible end points.
3. Calculate the sum of the current subarray.
4. Check if this sum is divisible by `K`.
5. Count the number of subarrays where the sum is divisible by `K`.

### Time Complexity
- **O(n^2)**: Double loop over the array, calculating the sum for each subarray.

### Space Complexity
- **O(1)**: No additional space required.

```javascript
function subarraysDivByK(A, K) {
    let count = 0;
    for (let start = 0; start < A.length; start++) {
        let sum = 0;
        for (let end = start; end < A.length; end++) {
            sum += A[end]; // Incrementally add to the sum
            if (sum % K === 0) { // Check divisibility by K
                count++; // Increase count if condition met
            }
        }
    }
    return count; 
}
```

## Approach 2: Efficient Prefix Sum with Hash Map

### Intuition
Instead of checking every single subarray, we can use prefix sums to keep track of sums more efficiently. 
- The key insight here is that if `prefixSum[i] % K == prefixSum[j] % K`, then the subarray from `i+1` to `j` is divisible by `K`.

### Approach
1. Use a hash map to store the frequency of each remainder when prefix sum is divided by `K`.
2. Initialize the hash map with the key `0`, meaning that there's one way to have a sum that is already divisible by `K` (the empty subarray).
3. Iterate over the array, maintaining a running sum.
4. For each element, calculate its prefix sum modulo `K`.
5. If this remainder has been seen before, it means there are that many subarrays ending at the current element which are divisible by `K`.
6. Increment the frequency of this remainder in the hash map.

### Time Complexity
- **O(n)**: Single pass through the array.

### Space Complexity
- **O(K)**: Space used by the hash map to store counts of remainders.

```javascript
function subarraysDivByK(A, K) {
    let count = 0;
    let sum = 0;
    let remainderCount = new Map();
    remainderCount.set(0, 1); // There is one way to have a sum modulo K == 0

    for (let num of A) {
        sum += num; // Compute running sum
        let remainder = sum % K;
        // Handle negative remainders to make sure they're positive
        if (remainder < 0) remainder += K;
        if (remainderCount.has(remainder)) {
            count += remainderCount.get(remainder); // Previous occurrences contribute to the new count
        }
        remainderCount.set(remainder, (remainderCount.get(remainder) || 0) + 1); // Update map for the current remainder count
    }
    
    return count;
}
```

This approach efficiently finds the number of subarrays with sums divisible by `K`, utilizing the properties of prefix sums and modular arithmetic.

