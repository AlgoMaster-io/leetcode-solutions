# [LeetCode 528: Random Pick with Weight](https://leetcode.com/problems/random-pick-with-weight/)

## Approaches
- [Approach 1: Prefix Sum with Linear Scan](#approach-1-prefix-sum-with-linear-scan)
- [Approach 2: Prefix Sum with Binary Search](#approach-2-prefix-sum-with-binary-search)

---

## Approach 1: Prefix Sum with Linear Scan

### Intuition
To solve this problem, we need a method to pick an index based on a given array of weights so that the probability of picking an element is proportional to its weight. The simplest way is to construct a prefix sum array. The prefix sum array allows us to summarize all elements' probabilities up to any index. Once we have the prefix sum array, we can generate a random number and find where it falls within this cumulative distribution.

### Steps:
1. Compute the prefix sum of the weights array.
2. Generate a random number between `1` and the total sum of weights.
3. Find the smallest index where the random number is less than or equal to the prefix sum value at that index. This can be done using a linear scan.

### Code

```typescript
class Solution {
    private prefixSums: number[] = [];
    private totalSum: number = 0;

    constructor(weights: number[]) {
        for (let weight of weights) {
            this.totalSum += weight;
            this.prefixSums.push(this.totalSum); // Constructing the prefix sum array
        }
    }

    pickIndex(): number {
        const random = Math.random() * this.totalSum; // Generate a random number within the range of total sum
        // Linear scan to find the first index where prefix sum is greater than the random number
        for (let i = 0; i < this.prefixSums.length; i++) {
            if (random < this.prefixSums[i]) {
                return i; // Return the index if the condition is met
            }
        }
        return -1; // Error case (should not reach here with valid input)
    }
}

```

### Complexity Analysis
- Time Complexity: 
  - Constructor: O(n)
  - `pickIndex`: O(n)
- Space Complexity: O(n)

## Approach 2: Prefix Sum with Binary Search

### Intuition
The linear scan to find the index can be optimized using binary search. Since the prefix sum array is sorted, binary search allows us to efficiently find the smallest index where the random number is less than or equal to the prefix sum value at that index.

### Steps:
1. Compute the prefix sum of the weights array (same as above).
2. Use binary search to efficiently locate where the random number falls within the prefix sum array.

### Code

```typescript
class Solution {
    private prefixSums: number[] = [];
    private totalSum: number = 0;

    constructor(weights: number[]) {
        for (let weight of weights) {
            this.totalSum += weight;
            this.prefixSums.push(this.totalSum); // Constructing the prefix sum array
        }
    }

    pickIndex(): number {
        const random = Math.random() * this.totalSum; // Generate a random number within the total range
        // Binary search to find the first index with a prefix sum greater than the random number
        let low = 0;
        let high = this.prefixSums.length - 1;
        while (low < high) {
            const mid = Math.floor((low + high) / 2);
            if (random < this.prefixSums[mid]) {
                high = mid; // Narrowing down the higher range
            } else {
                low = mid + 1; // Narrowing down the lower range
            }
        }
        return low; // Return the found index
    }
}
```

### Complexity Analysis
- Time Complexity: 
  - Constructor: O(n)
  - `pickIndex`: O(log n)
- Space Complexity: O(n)

By leveraging binary search, the pickIndex operation becomes significantly faster, especially with large data sizes, while maintaining the same space requirements.

