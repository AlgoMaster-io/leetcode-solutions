# [Leetcode 1512: Number of Good Pairs](https://leetcode.com/problems/number-of-good-pairs/)

## Approaches
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Optimized Using Hashmap](#approach-2-optimized-using-hashmap)

### Approach 1: Brute Force

**Intuition:**

In this approach, we will use two nested loops to check every possible pair in the array to find pairs `(i, j)` where `nums[i] == nums[j]` and `i < j`. This method is straightforward but not efficient due to its quadratic time complexity.

**Algorithm:**

1. Initialize a variable `count` to zero to store the count of good pairs.
2. Use two nested loops where the outer loop runs from `0` to `n-1` and the inner loop from `i+1` to `n`.
3. For each pair `(i, j)`, check if `nums[i]` is equal to `nums[j]`.
4. If they are equal, increment the `count`.
5. Return the `count` after all pairs have been checked.

**Code:**

```typescript
function numIdenticalPairs(nums: number[]): number {
    let count = 0;
    // Outer loop runs through each element
    for (let i = 0; i < nums.length; i++) {
        // Inner loop checks pairs (i, j) where j > i
        for (let j = i + 1; j < nums.length; j++) {
            // If a pair is good, increment count
            if (nums[i] === nums[j]) {
                count++;
            }
        }
    }
    return count;
}
```

**Complexity Analysis:**

- **Time Complexity:** O(n^2), where n is the number of elements in the array. This is because we check every possible pair.
- **Space Complexity:** O(1), since no additional space proportional to the input size is used.

### Approach 2: Optimized Using Hashmap

**Intuition:**

The brute force approach can be optimized using a hashmap. If a number appears multiple times in the array, all pairs formed by this number can easily be counted using combination mathematics. We can achieve a significant reduction in time complexity by pre-storing counts of each number and calculating the number of good pairs as we traverse the array.

**Algorithm:**

1. Initialize a hashmap `countMap` to keep track of the frequency of each number.
2. Initialize a variable `count` to zero to keep the number of good pairs.
3. Iterate through the array:
   - For each number `num` at index `i`, check how many identical numbers have appeared before it using `countMap`.
   - Add this count to the `count` because each of these represents a good pair with the current number.
   - Update the `countMap` by incrementing the count for the current number.
4. Return the `count`.

**Code:**

```typescript
function numIdenticalPairs(nums: number[]): number {
    let countMap: { [key: number]: number } = {};
    let count = 0;
    // Iterate over each number in the array
    for (const num of nums) {
        // Check if the number has been seen before
        if (countMap[num]) {
            // Add to the count of good pairs the number of times this number was seen before
            count += countMap[num];
        }
        // Update the map with the current number's occurrence
        countMap[num] = (countMap[num] || 0) + 1;
    }
    return count;
}
```

**Complexity Analysis:**

- **Time Complexity:** O(n), where n is the number of elements in the array. We traverse the array once.
- **Space Complexity:** O(n), for storing the frequency of each number in the hashmap.

By reducing the processing from checking every pair to counting occurrences, this approach is significantly more efficient.

