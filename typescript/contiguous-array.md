# [Leetcode 525: Contiguous Array](https://leetcode.com/problems/contiguous-array/)

## Approaches
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: HashMap and Prefix Sum](#approach-2-hashmap-and-prefix-sum)

### Approach 1: Brute Force

#### Solution
The brute force approach involves considering all possible contiguous subarrays and checking if they have an equal number of 0s and 1s.

#### Intuition
- Iterate through all possible subarrays of the given array.
- For each subarray, count the number of 0s and 1s.
- If a subarray has the same number of 0s and 1s, calculate its length and keep track of the maximum length encountered.

This approach is quite inefficient due to its high time complexity because it involves checking all possible subarrays.

#### Time and Space Complexity
- **Time Complexity:** O(n^2), where n is the length of the array because it checks all possible pairs.
- **Space Complexity:** O(1) because we only use variables for counting.

```typescript
function findMaxLengthBruteForce(nums: number[]): number {
    let maxLength = 0;
    const n = nums.length;
    
    for (let start = 0; start < n; start++) {
        let zeroCount = 0;
        let oneCount = 0;
        
        for (let end = start; end < n; end++) {
            if (nums[end] === 0) {
                zeroCount++;
            } else {
                oneCount++;
            }
            
            // If counts are equal, update maxLength
            if (zeroCount === oneCount) {
                maxLength = Math.max(maxLength, end - start + 1);
            }
        }
    }
    
    return maxLength;
}
```

### Approach 2: HashMap and Prefix Sum

#### Solution
Utilize a hashmap to track the first occurrence of a particular prefix sum. Convert 0s to -1s to treat the problem as finding subarrays with a sum of 0.

#### Intuition
- Convert the input array by treating 0s as -1s. Our goal is to find subarrays with a sum equal to zero.
- Maintain a running sum and use a hashmap to store the first occurrence of each running sum.
- If a running sum has been seen before, it indicates that there is a subarray with a zero sum between the previous index and the current index.
- Calculate the length of such a subarray and update the maximum length if necessary.

#### Time and Space Complexity
- **Time Complexity:** O(n), where n is the length of the array, as it involves a single pass.
- **Space Complexity:** O(n), due to the hashmap storing prefix sums.

```typescript
function findMaxLength(nums: number[]): number {
    const map: Map<number, number> = new Map();
    map.set(0, -1); // Initialize the map with sum 0 at index -1.
    let maxLength = 0;
    let runningSum = 0;
    
    for (let i = 0; i < nums.length; i++) {
        // Treat 0 as -1 for balance calculation.
        runningSum += nums[i] === 0 ? -1 : 1;
        
        // If this running sum has been seen before
        if (map.has(runningSum)) {
            // Calculate the length of the subarray with equal 0s and 1s
            maxLength = Math.max(maxLength, i - map.get(runningSum));
        } else {
            // Store the first occurrence of this running sum
            map.set(runningSum, i);
        }
    }
    
    return maxLength;
}
```

The second approach is efficient and suitable for large inputs due to its linear time complexity.

