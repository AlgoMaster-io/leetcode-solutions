# [Leetcode 219: Contains Duplicate II](https://leetcode.com/problems/contains-duplicate-ii/)

## Approaches:
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Hash Map for Efficient Lookup](#approach-2-hash-map-for-efficient-lookup)

---

## Approach 1: Brute Force

### Intuition:
The simplest approach to solve this problem is to use a nested loop to check each pair of indices `(i, j)` in the array. If the elements are the same and the difference in indices is less than or equal to `k`, we return `true`.

#### Steps:
1. Use two nested loops to iterate over each pair of indices `(i, j)`.
2. For each pair, check if `nums[i]` is equal to `nums[j]` and if the absolute difference between `i` and `j` is less than or equal to `k`.
3. If so, return `true`.

This solution examines every possible pair and checks their indices, ensuring correctness but at the cost of efficiency.

#### Code:
```typescript
function containsNearbyDuplicate(nums: number[], k: number): boolean {
    for (let i = 0; i < nums.length; i++) {
        for (let j = i + 1; j < nums.length; j++) {
            // Check if elements are the same and indices difference is <= k
            if (nums[i] === nums[j] && Math.abs(i - j) <= k) {
                return true; // Duplicate found within range
            }
        }
    }
    return false; // No such duplicates found
}
```

#### Complexity:
- **Time Complexity:** `O(n^2)`, where `n` is the length of the array. It checks every pair of indices.
- **Space Complexity:** `O(1)`, as we are using constant extra space.

---

## Approach 2: Hash Map for Efficient Lookup

### Intuition:
To improve the efficiency, we can utilize a hash map to keep track of the last index of every number we have seen so far. This allows us to check in constant time whether the current element is a duplicate within the allowed index range.

#### Steps:
1. Create a hash map to store the value and its last index.
2. Traverse the array once. For each element, check if it exists in the hash map.
3. If it does, verify if the difference between the current index and the stored index is `<= k`.
4. If yes, return `true`.
5. Update the hash map with the current element and its index.
6. If we finish the loop without returning `true`, return `false`.

This approach offers a more efficient solution by tracking indices and quickly checking the conditions using a hash map.

#### Code:
```typescript
function containsNearbyDuplicate(nums: number[], k: number): boolean {
    const indexMap: Map<number, number> = new Map(); // Map to store value and last index

    for (let i = 0; i < nums.length; i++) {
        // Check if the current number is in the map and if it was previously within the range of k
        if (indexMap.has(nums[i])) {
            const lastIndex = indexMap.get(nums[i])!;
            if (i - lastIndex <= k) {
                return true; // Duplicate within k range found
            }
        }
        indexMap.set(nums[i], i); // Update or add the current number's index
    }
    return false; // No duplicates found within range
}
```

#### Complexity:
- **Time Complexity:** `O(n)`, where `n` is the number of elements in the array. We traverse the list once and perform constant time operations for each element.
- **Space Complexity:** `O(n)`, in the worst case, we might store every element's index in the map.

By using a hash map, we significantly reduce the time complexity from `O(n^2)` to `O(n)`, which is much more efficient for larger inputs.

