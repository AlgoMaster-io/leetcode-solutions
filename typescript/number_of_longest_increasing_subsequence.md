# 673. [Number of Longest Increasing Subsequence](https://leetcode.com/problems/number-of-longest-increasing-subsequence/)

## Approach 1: Dynamic Programming with Two Arrays

### Solution
typescript
```typescript
// Time Complexity: O(n^2)
// Space Complexity: O(n)
function findNumberOfLIS(nums: number[]): number {
    if (!nums || nums.length === 0) return 0;

    const n = nums.length;
    const lengths = new Array(n).fill(1); // lengths[i] will be the length of the longest ending in nums[i]
    const counts = new Array(n).fill(1);  // counts[i] will be the number of the longest ending in nums[i]

    for (let i = 0; i < n; i++) {
        for (let j = 0; j < i; j++) {
            if (nums[i] > nums[j]) {
                if (lengths[j] + 1 > lengths[i]) {
                    lengths[i] = lengths[j] + 1;
                    counts[i] = counts[j];
                } else if (lengths[j] + 1 === lengths[i]) {
                    counts[i] += counts[j];
                }
            }
        }
    }

    let maxLength = 0, numberOfLIS = 0;
    for (let length of lengths) {
        maxLength = Math.max(maxLength, length);
    }
    for (let i = 0; i < n; i++) {
        if (lengths[i] === maxLength) {
            numberOfLIS += counts[i];
        }
    }

    return numberOfLIS;
}
```

## Approach 2: Optimized Dynamic Programming with Fenwick Tree

### Solution
typescript
```typescript
// Time Complexity: O(n log n)
// Space Complexity: O(n)
class FenwickTree {
    private lengths: number[];
    private counts: number[];

    constructor(size: number) {
        this.lengths = new Array(size).fill(0);
        this.counts = new Array(size).fill(0);
    }

    query(index: number): [number, number] {
        let maxLength = 0, totalCounts = 0;
        for (; index > 0; index -= index & -index) {
            if (this.lengths[index] > maxLength) {
                maxLength = this.lengths[index];
                totalCounts = this.counts[index];
            } else if (this.lengths[index] === maxLength) {
                totalCounts += this.counts[index];
            }
        }
        return [maxLength, totalCounts];
    }

    update(index: number, length: number, count: number): void {
        for (; index < this.lengths.length; index += index & -index) {
            if (this.lengths[index] < length) {
                this.lengths[index] = length;
                this.counts[index] = count;
            } else if (this.lengths[index] === length) {
                this.counts[index] += count;
            }
        }
    }
}

function findNumberOfLIS(nums: number[]): number {
    if (!nums || nums.length === 0) return 0;

    const offset = 1;
    const set = new Set<number>();
    nums.forEach(num => set.add(num));

    let rank = 0;
    const numToIndex = new Map<number, number>();
    Array.from(set).sort((a, b) => a - b).forEach(num => numToIndex.set(num, ++rank));

    const fenwickTree = new FenwickTree(rank + offset);
    let maxLength = 0, result = 0;

    for (let num of nums) {
        const currentIndex = numToIndex.get(num)!;
        const [prevLength, prevCount] = fenwickTree.query(currentIndex - 1);
        const currentLength = prevLength + 1;
        const currentCount = Math.max(prevCount, 1);

        fenwickTree.update(currentIndex, currentLength, currentCount);

        if (currentLength > maxLength) {
            maxLength = currentLength;
            result = currentCount;
        } else if (currentLength === maxLength) {
            result += currentCount;
        }
    }

    return result;
}
```

