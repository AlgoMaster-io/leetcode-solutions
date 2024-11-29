# [493. Reverse Pairs](https://leetcode.com/problems/reverse-pairs/)

## Approach 1: Merge Sort (Divide and Conquer)

### Solution
```javascript
// Time Complexity: O(n log n)
// Space Complexity: O(n)

class Solution {
    reversePairs(nums) {
        if (!nums || nums.length < 2) {
            return 0;
        }
        return this.mergeSort(nums, 0, nums.length - 1);
    }

    mergeSort(nums, left, right) {
        if (left >= right) {
            return 0;
        }

        const mid = left + Math.floor((right - left) / 2);

        // Count reverse pairs in left and right halves, and across them
        let count = this.mergeSort(nums, left, mid) + this.mergeSort(nums, mid + 1, right);

        // Count cross reverse pairs
        let j = mid + 1;
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

    merge(nums, left, mid, right) {
        const temp = [];
        let i = left, j = mid + 1, k = 0;

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

        for (let m = 0; m < temp.length; m++) {
            nums[left + m] = temp[m];
        }
    }
}
```

## Approach 2: Binary Indexed Tree (Fenwick Tree)

### Solution
```javascript
// Time Complexity: O(n log n)
// Space Complexity: O(n)

class Solution {
    reversePairs(nums) {
        // Coordinate compression
        const set = new Set();
        nums.forEach(num => {
            set.add(num);
            set.add(num * 2);
        });

        const map = new Map();
        let rank = 1;
        Array.from(set).sort((a, b) => a - b).forEach(num => {
            map.set(num, rank++);
        });

        // Binary Indexed Tree
        const bit = new Array(rank).fill(0);
        let count = 0;

        for (let i = nums.length - 1; i >= 0; i--) {
            // Count elements smaller than nums[i]
            count += this.query(bit, map.get(nums[i]) - 1);

            // Add nums[i] * 2 to the BIT
            this.update(bit, map.get(nums[i] * 2), 1);
        }

        return count;
    }

    update(bit, index, delta) {
        while (index < bit.length) {
            bit[index] += delta;
            index += index & -index;
        }
    }

    query(bit, index) {
        let sum = 0;
        while (index > 0) {
            sum += bit[index];
            index -= index & -index;
        }
        return sum;
    }
}
```

