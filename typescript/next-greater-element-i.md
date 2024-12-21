# [Leetcode 496: Next Greater Element I](https://leetcode.com/problems/next-greater-element-i/)

## Approaches
- [Brute Force Approach](#brute-force-approach)
- [Stack-based Optimized Approach](#stack-based-optimized-approach)

---

### Brute Force Approach

#### Intuition:
In this approach, we iterate over each element in `nums1` and try to find the next greater element for it in the array `nums2`. For every element in `nums1`, we look up its position in `nums2` and then proceed to search for the next greater element linearly.

#### Steps:
1. Create a map to store the index of each element in `nums2`.
2. For each element in `nums1`, locate its position in `nums2`.
3. From this position, look forward in `nums2` for the next greater element.
4. If found, note it down, otherwise, return -1 for that element.

#### Time and Space Complexity:
- **Time Complexity:** O(m * n), where m is the size of `nums1` and n is the size of `nums2`.
- **Space Complexity:** O(1).

#### Code:
```typescript
function nextGreaterElement(nums1: number[], nums2: number[]): number[] {
    // Create a result array initialized with -1
    const result: number[] = Array(nums1.length).fill(-1);

    // Map to store the index of each number in nums2
    const indexMap: Map<number, number> = new Map();
    nums2.forEach((num, idx) => indexMap.set(num, idx));

    // Iterate through each number in nums1
    for (let i = 0; i < nums1.length; i++) {
        const num = nums1[i];
        const startIndex = indexMap.get(num);

        // Search for the next greater element in the rest of nums2
        for (let j = startIndex! + 1; j < nums2.length; j++) {
            if (nums2[j] > num) {
                result[i] = nums2[j];
                break;
            }
        }
    }
    
    return result;
}
```

### Stack-based Optimized Approach

#### Intuition:
Utilizing a stack helps in efficiently finding the next greater element. This approach scans `nums2` only once and processes each item using the stack to keep track of potential "next greater" elements. This technique reduces redundant comparisons by leveraging the LIFO structure of the stack to track the order of elements.

#### Steps:
1. Initialize a stack and a map `nextGreaterMap` to store each element of `nums2` and its next greater value.
2. Traverse `nums2` from left to right.
3. For each element, pop items from the stack while the current element is greater than the top of the stack. It means the current element is the next greater for the popped elements.
4. Save these greater element relationships in `nextGreaterMap`.
5. Push the current element onto the stack.
6. For elements that remain in the stack after processing `nums2`, there is no next greater element, so they map to -1.
7. Construct the result for `nums1` by looking up the next greater element from `nextGreaterMap`.

#### Time and Space Complexity:
- **Time Complexity:** O(n + m), where n is the size of `nums2` and m is the size of `nums1`.
- **Space Complexity:** O(n), due to the stack and hashmap usage.

#### Code:
```typescript
function nextGreaterElement(nums1: number[], nums2: number[]): number[] {
    const nextGreaterMap: Map<number, number> = new Map();
    const stack: number[] = [];

    // Process each element in nums2
    for (const num of nums2) {
        // Resolve next greater elements for numbers in the stack
        while (stack.length > 0 && stack[stack.length - 1] < num) {
            const smallerNum = stack.pop()!;
            nextGreaterMap.set(smallerNum, num);
        }
        // Push current element to stack
        stack.push(num);
    }

    // Remaining elements in the stack do not have a greater element
    while (stack.length > 0) {
        const noGreaterNum = stack.pop()!;
        nextGreaterMap.set(noGreaterNum, -1);
    }

    // Build result array for nums1
    return nums1.map(num => nextGreaterMap.get(num) || -1);
}
```

This approach efficiently reduces time complexity and leverages the stack data structure for optimal performance.

