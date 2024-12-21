# [Leetcode 169: Majority Element](https://leetcode.com/problems/majority-element/)

## Approaches
- [Approach 1: Hash Map](#approach-1-hash-map)
- [Approach 2: Sorting](#approach-2-sorting)
- [Approach 3: Moore's Voting Algorithm](#approach-3-moores-voting-algorithm)

### Approach 1: Hash Map

**Intuition:**

The problem requires finding the majority element, which appears more than `n/2` times. A straightforward approach is to use a hash map to count the occurrences of each element. The element that has a count greater than `n/2` is our answer.

**Steps:**
1. Initialize an empty map to keep track of the count of each element.
2. Iterate over the array, updating the count of each element in the map.
3. Check if any element's count exceeds `n/2`. Return that element.

**TypeScript Code:**

```typescript
function majorityElement(nums: number[]): number {
    // Step 1: Create a map to track the count of each element
    const countMap: Map<number, number> = new Map();
    const majorityCount: number = Math.floor(nums.length / 2);

    // Step 2: Count each element
    for (let num of nums) {
        const count = (countMap.get(num) || 0) + 1;
        countMap.set(num, count);

        // Step 3: Check if current element is the majority element
        if (count > majorityCount) {
            return num;
        }
    }
    
    // Default return for typescript
    return -1; 
}
```

**Time Complexity:** `O(n)` - We iterate over the list once.

**Space Complexity:** `O(n)` - In the worst case, we store all elements in the map.

---

### Approach 2: Sorting

**Intuition:**

When sorted, the majority element will definitely be present at the position `n/2` because it appears more than half the time. So, the element at this position in the sorted array is our answer.

**Steps:**
1. Sort the array.
2. Return the element at index `n/2`.

**TypeScript Code:**

```typescript
function majorityElement(nums: number[]): number {
    // Step 1: Sort the array
    nums.sort((a, b) => a - b);
    
    // Step 2: Return the element at n/2 index
    return nums[Math.floor(nums.length / 2)];
}
```

**Time Complexity:** `O(n log n)` - Sorting the array takes `O(n log n)` time.

**Space Complexity:** `O(1)` - If we consider in-place sorting. Otherwise, `O(n)` for space used by the sorting algorithm.

---

### Approach 3: Moore's Voting Algorithm

**Intuition:**

This algorithm utilizes the fact that the majority element appears more than `n/2` times, thus outweighing all other elements combined. As we traverse the list, we maintain a count and a candidate. We increment the count for the current candidate if it matches the current number, or decrement it otherwise. If the count drops to zero, we change our candidate to the current number.

**Steps:**
1. Initialize a candidate and set a count to zero.
2. Iterate through each element:
   - If count is zero, update candidate to current number.
   - Adjust count based on whether current number equals candidate.
3. The candidate at the end is the majority element.

**TypeScript Code:**

```typescript
function majorityElement(nums: number[]): number {
    // Step 1: Initialize candidate and count
    let candidate: number | null = null;
    let count: number = 0;
    
    // Step 2 & 3: Traverse the array to find the candidate
    for (let num of nums) {
        if (count === 0) {
            candidate = num;
        }
        count += (num === candidate) ? 1 : -1;
    }
    
    // Candidate is the majority element 
    return candidate as number;
}
```

**Time Complexity:** `O(n)` - Single pass through the array.

**Space Complexity:** `O(1)` - No additional space used for computations.

