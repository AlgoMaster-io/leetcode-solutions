# [167. Two Sum II - Input Array Is Sorted](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/)

## Approach 1: Binary Search for the Complement

### Solution
typescript
```typescript
// Time Complexity: O(n log n)
// Space Complexity: O(1)
class Solution {
    twoSum(numbers: number[], target: number): number[] {
        for (let i = 0; i < numbers.length; i++) {
            const complement = target - numbers[i];
            const index = this.binarySearch(numbers, i + 1, numbers.length - 1, complement);

            if (index !== -1) {
                return [i + 1, index + 1]; // 1-based index
            }
        }

        return [-1, -1]; // No solution found
    }

    private binarySearch(numbers: number[], left: number, right: number, target: number): number {
        while (left <= right) {
            const mid = left + Math.floor((right - left) / 2);
            if (numbers[mid] === target) {
                return mid;
            } else if (numbers[mid] < target) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
        return -1; // Target not found
    }
}
```

## Approach 2: HashMap (For Non-Sorted Input)

### Solution
typescript
```typescript
// Time Complexity: O(n)
// Space Complexity: O(n)
class Solution {
    twoSum(numbers: number[], target: number): number[] {
        const map: Map<number, number> = new Map();

        for (let i = 0; i < numbers.length; i++) {
            const complement = target - numbers[i];

            if (map.has(complement)) {
                return [map.get(complement)! + 1, i + 1]; // 1-based index
            }

            map.set(numbers[i], i);
        }

        return [-1, -1]; // No solution found
    }
}
```

## Approach 3: Two Pointers

### Solution
typescript
```typescript
// Time Complexity: O(n)
// Space Complexity: O(1)
class Solution {
    twoSum(numbers: number[], target: number): number[] {
        let left = 0; // Start pointer
        let right = numbers.length - 1; // End pointer

        while (left < right) {
            const sum = numbers[left] + numbers[right];

            if (sum === target) {
                return [left + 1, right + 1]; // 1-based index
            } else if (sum < target) {
                left++; // Move left pointer to increase sum
            } else {
                right--; // Move right pointer to decrease sum
            }
        }

        return [-1, -1]; // No solution found
    }
}
```

