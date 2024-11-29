# 315. [Count of Smaller Numbers After Self](https://leetcode.com/problems/count-of-smaller-numbers-after-self/)

## Approach 1: Brute Force

### Solution
```javascript
// Time Complexity: O(n^2)
// Space Complexity: O(n)
class Solution {
    countSmaller(nums) {
        let result = [];
        
        // Traverse each element and count smaller numbers to its right
        for (let i = 0; i < nums.length; i++) {
            let count = 0;
            for (let j = i + 1; j < nums.length; j++) {
                if (nums[j] < nums[i]) {
                    count++;
                }
            }
            result.push(count);
        }
        
        return result;
    }
}
```

## Approach 2: Binary Indexed Tree (Fenwick Tree)

### Solution
```javascript
// Time Complexity: O(n log n)
// Space Complexity: O(n)
class Solution {
    countSmaller(nums) {
        let result = new Array(nums.length);
        let sortedNums = [...nums].sort((a, b) => a - b);
        let fenwickTree = new Array(nums.length + 1).fill(0);
        
        for (let i = nums.length - 1; i >= 0; i--) {
            let rank = this.binarySearch(sortedNums, nums[i]) + 1;
            result[i] = this.query(rank - 1, fenwickTree);
            this.update(rank, 1, fenwickTree);
        }
        
        return result;
    }
    
    binarySearch(arr, target) {
        let left = 0, right = arr.length - 1;
        while (left <= right) {
            let mid = Math.floor((left + right) / 2);
            if (arr[mid] < target) left = mid + 1;
            else if (arr[mid] > target) right = mid - 1;
            else return mid;
        }
        return left;
    }

    update(index, value, fenwickTree) {
        while (index < fenwickTree.length) {
            fenwickTree[index] += value;
            index += index & -index;
        }
    }

    query(index, fenwickTree) {
        let sum = 0;
        while (index > 0) {
            sum += fenwickTree[index];
            index -= index & -index;
        }
        return sum;
    }
}
```

## Approach 3: Merge Sort

### Solution
```javascript
// Time Complexity: O(n log n)
// Space Complexity: O(n)
class Solution {
    constructor() {
        this.counts = [];
    }

    countSmaller(nums) {
        this.counts = new Array(nums.length).fill(0);
        let indices = new Array(nums.length);
        
        for (let i = 0; i < nums.length; i++) {
            indices[i] = i;
        }
        
        this.mergeSort(nums, indices, 0, nums.length - 1);
        
        return this.counts;
    }
    
    mergeSort(nums, indices, left, right) {
        if (left >= right) return;
        
        let mid = left + Math.floor((right - left) / 2);
        this.mergeSort(nums, indices, left, mid);
        this.mergeSort(nums, indices, mid + 1, right);
        
        this.merge(nums, indices, left, mid, right);
    }
    
    merge(nums, indices, left, mid, right) {
        let tempIndices = [];
        let i = left, j = mid + 1, smallerCount = 0;
        
        while (i <= mid && j <= right) {
            if (nums[indices[i]] <= nums[indices[j]]) {
                this.counts[indices[i]] += smallerCount;
                tempIndices.push(indices[i++]);
            } else {
                smallerCount++;
                tempIndices.push(indices[j++]);
            }
        }
        
        while (i <= mid) {
            this.counts[indices[i]] += smallerCount;
            tempIndices.push(indices[i++]);
        }
        while (j <= right) {
            tempIndices.push(indices[j++]);
        }
        
        for (let k = left; k <= right; k++) {
            indices[k] = tempIndices[k - left];
        }
    }
}
```

