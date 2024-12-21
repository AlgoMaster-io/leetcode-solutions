# [Leetcode 493: Reverse Pairs](https://leetcode.com/problems/reverse-pairs)

## Table of Approaches
1. [Brute Force Approach](#brute-force-approach)
2. [Merge Sort Based Approach](#merge-sort-based-approach)
3. [Fenwick Tree Approach](#fenwick-tree-approach)

## Brute Force Approach

### Intuition
The brute force method involves iterating over each pair of indices `(i, j)` in the array where `i < j` and checking if a reverse pair condition `nums[i] > 2 * nums[j]` holds. While this method is straightforward, it is inefficient for larger arrays as it requires checking every possible pair.

### Implementation
```python
def reversePairs(nums):
    count = 0
    n = len(nums)
    for i in range(n):
        for j in range(i+1, n):
            if nums[i] > 2 * nums[j]:
                count += 1
    return count
```

### Time Complexity
- **Time Complexity**: O(n^2) where `n` is the length of the array since we check each pair of numbers.
- **Space Complexity**: O(1) as we are not using any extra space.

---

## Merge Sort Based Approach

### Intuition
This approach utilizes the merge sort algorithm to count reverse pairs during the merge step. The key idea is that if a number in the left sorted half fulfills the reverse condition with a number in the right sorted half, all subsequent numbers in the right half (due to being sorted) will also form reverse pairs. This allows us to count multiple pairs in a single go.

### Implementation
```python
def mergeSort(nums, temp, left, right):
    if left >= right:
        return 0
    
    mid = (left + right) // 2
    count = mergeSort(nums, temp, left, mid) + mergeSort(nums, temp, mid + 1, right)
    
    j = mid + 1
    for i in range(left, mid + 1):
        while j <= right and nums[i] > 2 * nums[j]:
            j += 1
        count += j - (mid + 1)
    
    merge(nums, temp, left, mid, right)
    return count

def merge(nums, temp, left, mid, right):
    i, j, k = left, mid + 1, left
    while i <= mid and j <= right:
        if nums[i] <= nums[j]:
            temp[k] = nums[i]
            i += 1
        else:
            temp[k] = nums[j]
            j += 1
        k += 1
        
    while i <= mid:
        temp[k] = nums[i]
        k += 1
        i += 1
        
    while j <= right:
        temp[k] = nums[j]
        k += 1
        j += 1
        
    for i in range(left, right + 1):
        nums[i] = temp[i]

def reversePairs(nums):
    if not nums:
        return 0
    temp = [0] * len(nums)
    return mergeSort(nums, temp, 0, len(nums) - 1)
```

### Time Complexity
- **Time Complexity**: O(n log n) due to the modified merge sort.
- **Space Complexity**: O(n) for the temporary array used during the merge process.

---

## Fenwick Tree Approach

### Intuition
The Fenwick Tree (or Binary Indexed Tree) can also be used to efficiently count reverse pairs. This method involves compressing coordinates and using the tree to accumulate counts of previous elements that fulfill the reverse condition.

### Implementation
```python
def reversePairs(nums):
    def add(bit, idx, val):
        while idx < len(bit):
            bit[idx] += val
            idx += idx & -idx

    def sum(bit, idx):
        total = 0
        while idx > 0:
            total += bit[idx]
            idx -= idx & -idx
        return total

    # Coordinate compress the array
    sorted_nums = sorted(set(nums + [2 * x for x in nums]))
    rank = {v: i + 1 for i, v in enumerate(sorted_nums)}

    bit = [0] * (len(rank) + 1)
    count = 0
    for num in reversed(nums):
        count += sum(bit, rank[num] - 1)
        add(bit, rank[2 * num], 1)
    
    return count
```

### Time Complexity
- **Time Complexity**: O(n log n) for the sorting and log queries in the Fenwick Tree.
- **Space Complexity**: O(n) due to the Fenwick Tree and ranking dictionary.

Each approach presented here offers a different trade-off between simplicity and efficiency, allowing for a step-by-step refinement toward optimal solutions for the problem of counting reverse pairs.

