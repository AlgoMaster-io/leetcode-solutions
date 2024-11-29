# 673. [Number of Longest Increasing Subsequence](https://leetcode.com/problems/number-of-longest-increasing-subsequence/)

## Approach 1: Dynamic Programming with Two Arrays

### Solution
```javascript
// Time Complexity: O(n^2)
// Space Complexity: O(n)
var findNumberOfLIS = function(nums) {
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
    maxLength = Math.max(...lengths);
    for (let i = 0; i < n; i++) {
        if (lengths[i] === maxLength) {
            numberOfLIS += counts[i];
        }
    }

    return numberOfLIS;
};
```

## Approach 2: Optimized Dynamic Programming with Fenwick Tree

### Solution
```javascript
// Time Complexity: O(n log n)
// Space Complexity: O(n)
class FenwickTree {
    constructor(size) {
        this.lengths = new Array(size).fill(0);
        this.counts = new Array(size).fill(0);
    }

    query(index) {
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

    update(index, length, count) {
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

var findNumberOfLIS = function(nums) {
    if (!nums || nums.length === 0) return 0;
    
    const offset = 1;
    const uniqueNums = new Set(nums);
    const sortedUniqueNums = Array.from(uniqueNums).sort((a, b) => a - b);

    const numToIndex = new Map();
    sortedUniqueNums.forEach((num, idx) => numToIndex.set(num, idx + offset));

    const fenwickTree = new FenwickTree(sortedUniqueNums.length + offset);
    let maxLength = 0, result = 0;

    nums.forEach(num => {
        const currentIndex = numToIndex.get(num);
        const [previousLength, previousCount] = fenwickTree.query(currentIndex - 1);
        const currentLength = previousLength + 1;
        const currentCount = Math.max(previousCount, 1);

        fenwickTree.update(currentIndex, currentLength, currentCount);

        if (currentLength > maxLength) {
            maxLength = currentLength;
            result = currentCount;
        } else if (currentLength === maxLength) {
            result += currentCount;
        }
    });

    return result;
};
```

