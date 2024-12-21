# [Leetcode 974: Subarray Sums Divisible by K](https://leetcode.com/problems/subarray-sums-divisible-by-k/)

## Approaches
- [Brute Force Approach](#brute-force-approach)
- [Optimized Approach Using Remainder and HashMap](#optimized-approach-using-remainder-and-hashmap)

### Brute Force Approach

**Intuition:**

The most straightforward way to solve the problem is to consider every possible subarray of the given array. For each subarray, compute its sum and check if it’s divisible by `K`. This approach is simple and works fine for small input sizes.

**Steps:**

1. Initialize a counter to zero which will store the number of subarrays satisfying the condition.
2. Iterate over all possible starting points of subarrays.
3. For each starting point, iterate over all possible ending points.
4. Calculate the sum of the current subarray.
5. Check if this sum is divisible by `K`. If so, increment the counter.
6. Return the counter after checking all subarrays.

**Code:**

```java
public int subarraysDivByK(int[] nums, int K) {
    int count = 0;

    // Iterate over each starting point for subarray
    for (int start = 0; start < nums.length; start++) {
        int sum = 0;
        // Iterate over the ending points for subarrays starting with 'start'
        for (int end = start; end < nums.length; end++) {
            sum += nums[end];
            // Check divisibility condition
            if (sum % K == 0) {
                count++;
            }
        }
    }

    return count;
}
```

**Time Complexity:**

- The time complexity of this approach is O(N^2) because we are considering all subarrays.

**Space Complexity:**

- The space complexity is O(1) since we're only using a constant amount of additional space.

---

### Optimized Approach Using Remainder and HashMap

**Intuition:**

The brute force approach works but is inefficient for larger arrays. We can optimize this by leveraging the properties of prefix sums and remainders. If the prefix sum up to `i` is `sum[0..i]` and prefix sum up to `j` is `sum[0..j]`, and both `sum[0..i] % K` and `sum[0..j] % K` are equal, then the subarray `sum[i+1..j]` is divisible by `K`.

We can use a HashMap to keep track of all seen remainder counts, making it easier to find the potential starting points for subarrays that end at each position in the array.

**Steps:**

1. Initialize a HashMap to store the count of each remainder.
2. Keep a running sum of the elements.
3. Calculate the remainder of the current sum by `K`.
4. If this remainder has been seen before, it means there's a subarray ending at the current position with a sum divisible by `K`.
5. Update the count of subarrays.
6. Update the remainder's count in the HashMap.

**Code:**

```java
public int subarraysDivByK(int[] nums, int K) {
    Map<Integer, Integer> remainderCount = new HashMap<>();
    remainderCount.put(0, 1);  // Base case for subarrays directly divisible by K

    int sum = 0, count = 0;
    
    for (int num : nums) {
        sum += num;
        
        // Compute remainder of the current cumulative sum
        int remainder = sum % K;
        // Handle negative remainders by converting them into a positive range [0,K-1]
        if (remainder < 0) remainder += K;
        
        // If we have seen this remainder before, then there exist 'remainderCount.get(remainder)' subarrays ending at current index
        if (remainderCount.containsKey(remainder)) {
            count += remainderCount.get(remainder);
        }

        // Update the remainder count in the map
        remainderCount.put(remainder, remainderCount.getOrDefault(remainder, 0) + 1);
    }

    return count;
}
```

**Time Complexity:**

- The time complexity of this approach is O(N) as we traverse the array once.

**Space Complexity:**

- The space complexity is O(K) because the hash map will contain at most `K` distinct remainders.

