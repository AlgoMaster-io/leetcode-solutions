# [315. Count of Smaller Numbers After Self](https://leetcode.com/problems/count-of-smaller-numbers-after-self/)

In this problem, we need to find out, for each number in a given array, how many numbers to the right of it are smaller than it. Let's explore different approaches to solve this:

## Approaches
1. [Brute Force Approach](#brute-force-approach)
2. [Binary Search and Insertion](#binary-search-and-insertion)
3. [Merge Sort with Index Tracking](#merge-sort-with-index-tracking)
4. [Fenwick Tree (Binary Indexed Tree)](#fenwick-tree-binary-indexed-tree)

### Brute Force Approach

**Intuition:**  
The simplest approach is to use two loops. For each element, iterate through all subsequent elements to count how many of them are smaller. Though this is inefficient, it's straightforward to implement and understand.

**Time Complexity:**  
- O(n^2), where n is the number of elements in the array.
  
**Space Complexity:**  
- O(1), excluding the space used for the answer.

```typescript
function countSmallerBruteForce(nums: number[]): number[] {
    const result: number[] = Array(nums.length).fill(0);

    for (let i = 0; i < nums.length; i++) {
        // For each number, count smaller numbers to its right
        for (let j = i + 1; j < nums.length; j++) {
            if (nums[j] < nums[i]) {
                result[i]++;
            }
        }
    }

    return result;
}
```
- **Explanation:** We iterate over each element `i`, and for each `i`, iterate over elements to its right checking if they are smaller.

### Binary Search and Insertion

**Intuition:**  
For slightly better efficiency, we can process the array from right to left. As we encounter each number, insert it into a sorted array (keeping that array sorted), and use binary search to find how many numbers are smaller and to the right.

**Time Complexity:**  
- O(n log n), where n is the number of elements (on average, with insertion in a balanced set).
  
**Space Complexity:**  
- O(n), for storing elements in sorted order.

```typescript
function countSmallerBinarySearch(nums: number[]): number[] {
    const sorted: number[] = [];
    const result: number[] = Array(nums.length).fill(0);

    for (let i = nums.length - 1; i >= 0; i--) {
        // Finding the insertion position using binary search
        let left = 0;
        let right = sorted.length;
        while (left < right) {
            const mid = Math.floor((left + right) / 2);
            if (sorted[mid] >= nums[i]) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }

        // Inserting at the correct position
        sorted.splice(left, 0, nums[i]);

        // The left index is how many numbers are smaller and to the right
        result[i] = left;
    }

    return result;
}
```
- **Explanation:** Use binary search to determine the insertion point for each number in a dynamically growing sorted list. The position gives the count of smaller numbers to the right.

### Merge Sort with Index Tracking

**Intuition:**  
Use a modified merge sort to count inversions. When merging, count how many numbers from the right are less than numbers from the left. This counting gives us the required number of smaller elements to the right.

**Time Complexity:**  
- O(n log n), where n is the length of the array.
  
**Space Complexity:**  
- O(n), for temporary arrays used during merge operations.

```typescript
function countSmallerMergeSort(nums: number[]): number[] {
    const n = nums.length;
    const result: number[] = Array(n).fill(0);
    const indices: number[] = Array.from(nums.keys());

    function mergeSort(start: number, end: number): void {
        if (end <= start) return;

        const mid = Math.floor((start + end) / 2);
        mergeSort(start, mid);
        mergeSort(mid + 1, end);

        const temp: number[] = [];
        let left = start, right = mid + 1, rightCounter = 0;

        while (left <= mid || right <= end) {
            if (right > end || (left <= mid && nums[indices[left]] <= nums[indices[right]])) {
                result[indices[left]] += rightCounter;
                temp.push(indices[left++]);
            } else {
                temp.push(indices[right++]);
                rightCounter++;
            }
        }

        for (let i = start; i <= end; i++) {
            indices[i] = temp[i - start];
        }
    }

    mergeSort(0, n - 1);
    return result;
}
```
- **Explanation:** Merge sort divides the array and counts how many times an element from the right subarray is less than an element from the left subarray during the merge step.

### Fenwick Tree (Binary Indexed Tree)

**Intuition:**  
Use a Fenwick Tree to efficiently update frequencies and query counts of elements smaller than a given number, accounting for all elements seen so far from right to left.

**Time Complexity:**  
- O(n log m), where n is the length of the array and m is the range of numbers.
  
**Space Complexity:**  
- O(m), dependent on the number of unique elements (or scale).

```typescript
class FenwickTree {
    tree: number[];
    
    constructor(size: number) {
        this.tree = Array(size + 1).fill(0);
    }
    
    update(index: number, delta: number): void {
        while (index < this.tree.length) {
            this.tree[index] += delta;
            index += index & -index;
        }
    }
    
    query(index: number): number {
        let sum = 0;
        while (index > 0) {
            sum += this.tree[index];
            index -= index & -index;
        }
        return sum;
    }
}

function countSmallerFenwickTree(nums: number[]): number[] {
    // Coordinate compression to map elements to their rank order
    const sortedUnique = Array.from(new Set(nums)).sort((a, b) => a - b);
    const rankMap = new Map<number, number>();
    sortedUnique.forEach((value, index) => {
        rankMap.set(value, index + 1); // 1-indexed Fenwick Tree
    });

    const fenwick = new FenwickTree(sortedUnique.length);
    const result: number[] = Array(nums.length).fill(0);

    for (let i = nums.length - 1; i >= 0; i--) {
        const rank = rankMap.get(nums[i])!;
        result[i] = fenwick.query(rank - 1);
        fenwick.update(rank, 1);
    }

    return result;
}
```
- **Explanation:** Fenwick Tree helps us efficiently manage and query elements by using coordinate compression and updating frequencies. The process efficiently captures the number of smaller elements when processing from right to left.

