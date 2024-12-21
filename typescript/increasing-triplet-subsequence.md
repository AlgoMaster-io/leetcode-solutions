# [Leetcode 334: Increasing Triplet Subsequence](https://leetcode.com/problems/increasing-triplet-subsequence/)

## Approaches
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Two Pointers with Linear Scan](#approach-2-two-pointers-with-linear-scan)

### Approach 1: Brute Force

**Intuition**:  
The simplest way to solve the problem is to test every combination of triples in the array to check if they form an increasing triplet subsequence. This approach lacks efficiency but is a good starting point to understand the problem.

**Algorithm**:
1. Traverse the array with three nested loops.
2. For each combination of three elements, check if they satisfy the condition `nums[i] < nums[j] < nums[k]` where `i < j < k`.
3. If such a combination exists, return `true`.
4. If no such triplet is found throughout the iterations, return `false`.

**Code**:
```typescript
function increasingTriplet(nums: number[]): boolean {
    // Loop through each possible triplet i, j, k with i < j < k
    for (let i = 0; i < nums.length - 2; i++) {
        for (let j = i + 1; j < nums.length - 1; j++) {
            for (let k = j + 1; k < nums.length; k++) {
                // Check if the triplet is increasing
                if (nums[i] < nums[j] && nums[j] < nums[k]) {
                    return true;
                }
            }
        }
    }
    return false;
}
```

**Time Complexity**: O(n^3)  
**Space Complexity**: O(1)

---

### Approach 2: Two Pointers with Linear Scan

**Intuition**:  
By maintaining two variables `first` and `second`, we can track the smallest and middle number found so far in an attempt to find the third number greater than `second`. This approach aims to confirm the existence of a triplet by leveraging the properties of an increasing subsequence.

**Algorithm**:
1. Initialize two variables `first` and `second` with `Infinity` to find the first two increasing numbers.
2. Iterate over each number in the array.
   - If the current number is less than or equal to `first`, update `first`.
   - Else if the current number is less than or equal to `second`, update `second`.
   - Otherwise, we found `nums[k]`, the third number which is greater than both `first` and `second`, thus return `true`.
3. If no such triplet is found, return `false`.

**Code**:
```typescript
function increasingTriplet(nums: number[]): boolean {
    let first = Infinity;
    let second = Infinity;
    
    for (const num of nums) {
        if (num <= first) {
            // Update the first smallest number
            first = num;
        } else if (num <= second) {
            // Update the second smallest when `first < num <= second`
            second = num;
        } else {
            // If we're here, it means `first < second < num`, thus we have an increasing triplet
            return true;
        }
    }
    return false;
}
```

**Time Complexity**: O(n) - We only iterate through the list once.  
**Space Complexity**: O(1) - No extra space used besides two variables.  

The second approach is significantly more efficient and demonstrates the power of reducing time complexity from O(n^3) to O(n) for large datasets.

