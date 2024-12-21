# [Leetcode 215: Kth Largest Element in an Array](https://leetcode.com/problems/kth-largest-element-in-an-array/)

## Approaches:
1. [Brute Force: Sort the Array](#approach-1-brute-force-sort-the-array)
2. [Heap: Min-Heap to Find Kth Largest](#approach-2-heap-min-heap-to-find-kth-largest)
3. [Quickselect Algorithm](#approach-3-quickselect-algorithm)

---

## Approach 1: Brute Force: Sort the Array

### Intuition:
The simplest way to find the kth largest element is to sort the array in descending order and simply return the kth element. Sorting will bring the largest elements to the beginning of the array, making it easy to access the kth largest.

### Steps:
- Sort the array in descending order.
- The kth largest element will be at index `k-1`.

### Code:
```javascript
var findKthLargest = function(nums, k) {
    // Sort the array in descending order
    nums.sort((a, b) => b - a);
    // Return the element at the k-1 index
    return nums[k - 1];
};
```

### Complexity:
- **Time Complexity**: O(n log n), where n is the number of elements in the array. This is due to the sorting operation.
- **Space Complexity**: O(1), or O(log n) if we consider the space used by the sorting algorithm implementation (it's typically done in-place).

---

## Approach 2: Heap: Min-Heap to Find Kth Largest

### Intuition:
Using a min-heap of size `k` allows us to efficiently track the largest `k` elements in the array. By the property of a min-heap, the smallest of these `k` elements will always be at the root, which gives us direct access to the `k`th largest element.

### Steps:
- Create an empty min-heap.
- Add elements from the array to the heap.
- If the heap size exceeds `k`, remove the smallest element.
- After processing all elements, the root of the heap will be the kth largest element.

### Code:
```javascript
var findKthLargest = function(nums, k) {
    let minHeap = new MinPriorityQueue(); // Create a min-heap using a priority queue

    for (let num of nums) {
        minHeap.enqueue(num); // Add each number to the heap
        if (minHeap.size() > k) {
            minHeap.dequeue(); // Remove the smallest element if we exceed size k
        }
    }

    return minHeap.front().element; // The root of the heap is the kth largest element
};
```

### Complexity:
- **Time Complexity**: O(n log k), maintaining the heap of size `k` takes logarithmic time.
- **Space Complexity**: O(k), due to the space required by the heap.

---

## Approach 3: Quickselect Algorithm

### Intuition:
The Quickselect algorithm is a variation of Quicksort and is highly efficient for this problem because it can select the kth smallest or largest element in linear average time. The idea involves partitioning the array such that elements on one side are either larger or smaller based on the pivot.

### Steps:
- Randomly choose a pivot and partition the array based on the pivot.
- Determine if the pivot is at the kth position. If yes, return the pivot.
- If the kth largest is in the right half, recursively apply quickselect on the right.
- Otherwise, recursively apply it on the left.

### Code:
```javascript
var findKthLargest = function(nums, k) {
    const quickselect = (left, right, kSmallest) => {
        if (left === right) return nums[left]; // If there's only one element
        let pivotIndex = partition(left, right); // Partition the array

        // The pivot is in its final sorted position
        if (kSmallest === pivotIndex) {
            return nums[kSmallest];
        } else if (kSmallest < pivotIndex) {
            return quickselect(left, pivotIndex - 1, kSmallest);
        } else {
            return quickselect(pivotIndex + 1, right, kSmallest);
        }
    };

    const partition = (left, right) => {
        let pivot = nums[right];
        let i = left;
        for (let j = left; j < right; j++) {
            if (nums[j] > pivot) {
                [nums[i], nums[j]] = [nums[j], nums[i]];
                i++;
            }
        }
        [nums[i], nums[right]] = [nums[right], nums[i]];
        return i;
    };

    // Kth largest is (n - k)th smallest with 0-based index
    return quickselect(0, nums.length - 1, nums.length - k);
};
```

### Complexity:
- **Time Complexity**: O(n) on average, as the size of the problem is roughly halved with each iteration.
- **Space Complexity**: O(1), excluding the recursion stack.

