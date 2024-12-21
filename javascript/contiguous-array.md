# [Leetcode 525: Contiguous Array](https://leetcode.com/problems/contiguous-array/)

## Approaches
1. [Brute Force](#approach-1-brute-force)
2. [HashMap with Cumulative Count](#approach-2-hashmap-with-cumulative-count)

---

## Approach 1: Brute Force

### Intuition:
The brute force approach involves checking each possible subarray to determine if it is balanced (contains the same number of zeros and ones). This can be achieved by calculating the count of zeros and ones for each subarray and confirming if they match.

### Algorithm:
1. Iterate over each possible starting index `i` of the subarray.
2. For each `i`, iterate over each possible ending index `j`.
3. Count the number of zeros and ones between index `i` and `j`.
4. If the count is equal, calculate the length of this subarray and compare it with the maximum length found so far.
5. Return the maximum length.

### Code:

```javascript
var findMaxLength = function(nums) {
    let maxLen = 0;
    const n = nums.length;
    
    // Iterate over each possible starting point
    for (let i = 0; i < n; i++) {
        // Initialize counts of zeros and ones
        let count0 = 0, count1 = 0;
        // Explore subarrays starting at `i`
        for (let j = i; j < n; j++) {
            // Count zeros and ones
            if (nums[j] === 0) {
                count0++;
            } else {
                count1++;
            }
            // Check if the subarray is balanced
            if (count0 === count1) {
                // Update max length
                maxLen = Math.max(maxLen, j - i + 1);
            }
        }
    }
    return maxLen;
};
```

### Time Complexity:
- O(n^2), where n is the number of elements in `nums`, as each pair of indices is explored.

### Space Complexity:
- O(1) additional space, since we utilize only a fixed number of extra space for counting.

---

## Approach 2: HashMap with Cumulative Count

### Intuition:
The optimal approach leverages the idea of transforming the problem into one of finding a subarray with zero net sum by replacing zeros with -1. We track the cumulative sums during iteration, noting the first index where each cumulative sum occurs. If the same sum occurs again, it indicates that the subarray between these indices is balanced.

### Algorithm:
1. Replace zeros with -1 to balance the idea of counting.
2. Track the cumulative sum as you iterate through the array.
3. Use a map to store the first occurrence of each cumulative sum.
4. If a cumulative sum is seen again, calculate the length of the subarray between those two occurrences.
5. Update the maximum length if this subarray is the longest found so far.
6. Return the maximum length.

### Code:

```javascript
var findMaxLength = function(nums) {
    let maxLen = 0, count = 0;
    const map = new Map();
    map.set(0, -1); // Initialize map with (cumulative sum, index) as (0, -1) to handle sum from the start
    
    for (let i = 0; i < nums.length; i++) {
        count += (nums[i] === 0 ? -1 : 1); // Replace 0 with -1 and accumulate
        
        if (map.has(count)) {
            // Calculate length of subarray with zero net sum
            maxLen = Math.max(maxLen, i - map.get(count));
        } else {
            // Store the first occurrence of this cumulative sum
            map.set(count, i);
        }
    }
    
    return maxLen;
};
```

### Time Complexity:
- O(n), where n is the number of elements in `nums`, as the array is traversed once.

### Space Complexity:
- O(n), due to the space used by the hashmap to store different cumulative sums and their indices.

With these two approaches, you can understand both the naive brute force method and the optimal method utilizing a hashmap for better time efficiency.

