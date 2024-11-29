# 260. [Single Number III](https://leetcode.com/problems/single-number-iii/)

## Approach 1: HashMap for Counting Frequencies

### Solution
typescript
```typescript
// Time Complexity: O(n)
// Space Complexity: O(n)

function singleNumber(nums: number[]): number[] {
    const countMap: Map<number, number> = new Map();

    // Count the frequency of each number
    for (let num of nums) {
        countMap.set(num, (countMap.get(num) || 0) + 1);
    }

    const result: number[] = [];
    // Find the numbers with frequency 1
    for (let num of countMap.keys()) {
        if (countMap.get(num) === 1) {
            result.push(num);
        }
    }

    return result;
}
```

## Approach 2: Bit Manipulation

### Solution
typescript
```typescript
// Time Complexity: O(n)
// Space Complexity: O(1)

function singleNumber(nums: number[]): number[] {
    let xor = 0;

    // XOR all numbers to get xor of the two unique numbers
    for (let num of nums) {
        xor ^= num;
    }

    // Find a set bit (rightmost) in xor to separate the numbers
    const diff = xor & -xor;

    const result: number[] = [0, 0];
    // Separate the numbers into two groups and XOR them respectively
    for (let num of nums) {
        if ((num & diff) === 0) {
            result[0] ^= num; // XOR for first group
        } else {
            result[1] ^= num; // XOR for second group
        }
    }

    return result;
}
```

