# 315. [Count of Smaller Numbers After Self](https://leetcode.com/problems/count-of-smaller-numbers-after-self/)

## Approach 1: Brute Force

### Solution
typescript
```typescript
// Time Complexity: O(n^2)
// Space Complexity: O(n)
function countSmaller(nums: number[]): number[] {
    const result: number[] = [];
    
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
```

## Approach 2: Binary Indexed Tree (Fenwick Tree)

### Solution
typescript
```typescript
// Time Complexity: O(n log n)
// Space Complexity: O(n)
function countSmaller(nums: number[]): number[] {
    const result: number[] = new Array(nums.length).fill(0);
    const sortedNums: number[] = [...nums].sort((a, b) => a - b);
    const fenwickTree: number[] = new Array(nums.length + 1).fill(0);

    // Traverse nums from right to left
    for (let i = nums.length - 1; i >= 0; i--) {
        const rank = sortedNums.indexOf(nums[i]) + 1;
        result[i] = fenwickQuery(rank - 1, fenwickTree);
        fenwickUpdate(rank, 1, fenwickTree);
    }

    return result;
}

// Update the Fenwick Tree
function fenwickUpdate(index: number, value: number, fenwickTree: number[]): void {
    while (index < fenwickTree.length) {
        fenwickTree[index] += value;
        index += index & -index;
    }
}

// Query the prefix sum in the Fenwick Tree
function fenwickQuery(index: number, fenwickTree: number[]): number {
    let sum = 0;
    while (index > 0) {
        sum += fenwickTree[index];
        index -= index & -index;
    }
    return sum;
}
```

## Approach 3: Merge Sort

### Solution
typescript
```typescript
// Time Complexity: O(n log n)
// Space Complexity: O(n)
class MergeSortSolution {
    private counts: number[];

    public countSmaller(nums: number[]): number[] {
        this.counts = new Array(nums.length).fill(0);
        const indices = Array.from({ length: nums.length }, (_, i) => i);
        
        // Perform merge sort
        this.mergeSort(nums, indices, 0, nums.length - 1);
        
        return this.counts;
    }
    
    // Perform merge sort and count smaller elements
    private mergeSort(nums: number[], indices: number[], left: number, right: number): void {
        if (left >= right) return;
        
        const mid = left + Math.floor((right - left) / 2);
        this.mergeSort(nums, indices, left, mid);
        this.mergeSort(nums, indices, mid + 1, right);
        
        this.merge(nums, indices, left, mid, right);
    }
    
    // Merge two sorted halves and update counts
    private merge(nums: number[], indices: number[], left: number, mid: number, right: number): void {
        const tempIndices: number[] = new Array(right - left + 1);
        let i = left, j = mid + 1, k = 0, smallerCount = 0;
        
        // Merge the two halves
        while (i <= mid && j <= right) {
            if (nums[indices[i]] <= nums[indices[j]]) {
                this.counts[indices[i]] += smallerCount;
                tempIndices[k++] = indices[i++];
            } else {
                smallerCount++;
                tempIndices[k++] = indices[j++];
            }
        }
        
        // Copy remaining elements
        while (i <= mid) {
            this.counts[indices[i]] += smallerCount;
            tempIndices[k++] = indices[i++];
        }
        while (j <= right) {
            tempIndices[k++] = indices[j++];
        }
        
        // Copy merged result back to indices
        for (let l = 0; l < tempIndices.length; l++) {
            indices[left + l] = tempIndices[l];
        }
    }
}
```

