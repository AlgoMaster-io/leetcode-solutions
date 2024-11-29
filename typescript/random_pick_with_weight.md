# 528. [Random Pick with Weight](https://leetcode.com/problems/random-pick-with-weight/)

## Approach 1: Linear Search for Picking (Alternative to Binary Search)

### Solution
typescript
```typescript
// Time Complexity: O(n) for initialization, O(n) for each pick
// Space Complexity: O(n)

class Solution {
    private prefixSums: number[];
    private totalSum: number;

    constructor(w: number[]) {
        this.prefixSums = new Array(w.length);
        
        // Build the prefix sum array
        this.prefixSums[0] = w[0];
        for (let i = 1; i < w.length; i++) {
            this.prefixSums[i] = this.prefixSums[i - 1] + w[i];
        }
        this.totalSum = this.prefixSums[this.prefixSums.length - 1];
    }

    pickIndex(): number {
        // Generate a random number between 1 and the total sum
        const target = Math.floor(Math.random() * this.totalSum) + 1;

        // Perform linear search to find the target index
        for (let i = 0; i < this.prefixSums.length; i++) {
            if (target <= this.prefixSums[i]) {
                return i; // Return the first index with a prefix sum >= target
            }
        }

        return -1; // Should not reach here
    }
}
```

## Approach 2: Prefix Sum Array and Binary Search

### Solution
typescript
```typescript
// Time Complexity: O(n) for initialization, O(log n) for each pick
// Space Complexity: O(n)

class Solution {
    private prefixSums: number[];
    private totalSum: number;

    constructor(w: number[]) {
        this.prefixSums = new Array(w.length);

        // Build the prefix sum array
        this.prefixSums[0] = w[0];
        for (let i = 1; i < w.length; i++) {
            this.prefixSums[i] = this.prefixSums[i - 1] + w[i];
        }
        this.totalSum = this.prefixSums[this.prefixSums.length - 1];
    }

    pickIndex(): number {
        // Generate a random number between 1 and the total sum
        const target = Math.floor(Math.random() * this.totalSum) + 1;

        // Use binary search to find the target index
        let start = 0, end = this.prefixSums.length - 1;
        while (start < end) {
            const mid = start + Math.floor((end - start) / 2);
            if (this.prefixSums[mid] < target) {
                start = mid + 1; // Narrow search to the right half
            } else {
                end = mid; // Narrow search to the left half
            }
        }

        return start; // Return the index corresponding to the random number
    }
}
```

