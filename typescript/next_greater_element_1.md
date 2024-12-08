# 496. [Next Greater Element I](https://leetcode.com/problems/next-greater-element-i/)

## Approach 1: Brute Force

### Solution
```typescript
// Time Complexity: O(n * m)
// Space Complexity: O(1)
function nextGreaterElement(nums1: number[], nums2: number[]): number[] {
    const result: number[] = new Array(nums1.length);

    // Iterate over each element in nums1
    for (let i = 0; i < nums1.length; i++) {
        const current = nums1[i];
        let indexInNums2 = -1;

        // Find the index of current in nums2
        for (let j = 0; j < nums2.length; j++) {
            if (nums2[j] === current) {
                indexInNums2 = j;
                break;
            }
        }

        // Look for the next greater element to the right in nums2
        result[i] = -1; // Default if no greater element is found
        for (let j = indexInNums2 + 1; j < nums2.length; j++) {
            if (nums2[j] > current) {
                result[i] = nums2[j];
                break;
            }
        }
    }
    return result;
}
```

## Approach 2: Using a Stack (Optimal Solution)

### Solution
```typescript
// Time Complexity: O(n + m)
// Space Complexity: O(m)
function nextGreaterElement(nums1: number[], nums2: number[]): number[] {
    const nextGreaterMap: Map<number, number> = new Map();
    const stack: number[] = [];

    // Traverse nums2 in reverse to populate the nextGreaterMap
    for (let num of nums2) {
        // Maintain the stack in decreasing order
        while (stack.length > 0 && stack[stack.length - 1] <= num) {
            stack.pop();
        }
        // If stack is not empty, the top of the stack is the next greater element
        nextGreaterMap.set(num, stack.length === 0 ? -1 : stack[stack.length - 1]);
        stack.push(num);
    }

    // Build the result for nums1 using the map
    const result: number[] = new Array(nums1.length);
    for (let i = 0; i < nums1.length; i++) {
        result[i] = nextGreaterMap.get(nums1[i])!;
    }

    return result;
}
```

