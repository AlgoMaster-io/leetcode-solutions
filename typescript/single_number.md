# 136. [Single Number](https://leetcode.com/problems/single-number/)

## Approach 1: Using HashMap

### Solution
typescript
```typescript
// Time Complexity: O(n)
// Space Complexity: O(n)
function singleNumber(nums: number[]): number {
    const countMap = new Map<number, number>();

    // Count the frequency of each number
    for (const num of nums) {
        countMap.set(num, (countMap.get(num) || 0) + 1);
    }

    // Find the number with frequency 1
    for (const num of nums) {
        if (countMap.get(num) === 1) {
            return num;
        }
    }

    return -1; // No unique number found
}
```

## Approach 2: Using Sorting

### Solution
typescript
```typescript
// Time Complexity: O(n log n)
// Space Complexity: O(1)
function singleNumber(nums: number[]): number {
    nums.sort((a, b) => a - b); // Sort the array

    // Traverse the array looking for unique element
    for (let i = 0; i < nums.length - 1; i += 2) {
        if (nums[i] !== nums[i + 1]) {
            return nums[i];
        }
    }

    return nums[nums.length - 1]; // The last element is the single number
}
```

## Approach 3: Using XOR

### Solution
typescript
```typescript
// Time Complexity: O(n)
// Space Complexity: O(1)
function singleNumber(nums: number[]): number {
    let result = 0;

    // XOR all elements, duplicate elements will cancel out
    for (const num of nums) {
        result ^= num;
    }

    return result; // The remaining single number
}
```

