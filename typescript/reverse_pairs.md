# [493. Reverse Pairs](https://leetcode.com/problems/reverse-pairs/)

## Approach 1: Merge Sort (Divide and Conquer)

### Solution
typescript
```typescript
// Time Complexity: O(n log n)
// Space Complexity: O(n)
class Solution {
    reversePairs(nums: number[]): number {
        if (nums == null || nums.length < 2) {
            return 0;
        }
        return this.mergeSort(nums, 0, nums.length - 1);
    }

    private mergeSort(nums: number[], left: number, right: number): number {
        if (left >= right) {
            return 0;
        }

        const mid: number = left + Math.floor((right - left) / 2);

        // Count reverse pairs in left and right halves, and across them
        let count: number = this.mergeSort(nums, left, mid) + this.mergeSort(nums, mid + 1, right);

        // Count cross reverse pairs
        let j: number = mid + 1;
        for (let i = left; i <= mid; i++) {
            while (j <= right && nums[i] > 2 * nums[j]) {
                j++;
            }
            count += (j - mid - 1);
        }

        // Merge the two halves
        this.merge(nums, left, mid, right);

        return count;
    }

    private merge(nums: number[], left: number, mid: number, right: number): void {
        const temp: number[] = new Array(right - left + 1);
        let i: number = left, j: number = mid + 1, k: number = 0;

        while (i <= mid && j <= right) {
            if (nums[i] <= nums[j]) {
                temp[k++] = nums[i++];
            } else {
                temp[k++] = nums[j++];
            }
        }

        while (i <= mid) {
            temp[k++] = nums[i++];
        }

        while (j <= right) {
            temp[k++] = nums[j++];
        }

        // Copy the sorted elements back into the original array
        for (let l = 0; l < temp.length; l++) {
            nums[left + l] = temp[l];
        }
    }
}
```

## Approach 2: Binary Indexed Tree (Fenwick Tree)

### Solution
typescript
```typescript
// Time Complexity: O(n log n)
// Space Complexity: O(n)
class Solution {
    reversePairs(nums: number[]): number {
        // Coordinate compression
        const set: Set<number> = new Set();
        for (const num of nums) {
            set.add(num);
            set.add(num * 2);
        }

        const map: Map<number, number> = new Map();
        let rank: number = 1;
        for (const num of Array.from(set).sort((a, b) => a - b)) {
            map.set(num, rank++);
        }

        // Binary Indexed Tree
        const bit: number[] = new Array(rank).fill(0);
        let count: number = 0;

        for (let i = nums.length - 1; i >= 0; i--) {
            // Count elements smaller than nums[i]
            count += this.query(bit, map.get(nums[i])! - 1);

            // Add nums[i] * 2 to the BIT
            this.update(bit, map.get(nums[i] * 2)!, 1);
        }

        return count;
    }

    private update(bit: number[], index: number, delta: number): void {
        while (index < bit.length) {
            bit[index] += delta;
            index += index & -index;
        }
    }

    private query(bit: number[], index: number): number {
        let sum: number = 0;
        while (index > 0) {
            sum += bit[index];
            index -= index & -index;
        }
        return sum;
    }
}
```

