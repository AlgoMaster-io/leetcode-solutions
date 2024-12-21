# [Smallest Range Covering Elements from K Lists](https://leetcode.com/problems/smallest-range-covering-elements-from-k-lists)

## Approaches
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Priority Queue (Min Heap)](#approach-2-priority-queue-min-heap)

---

## Approach 1: Brute Force

### Intuition
The brute force approach is fairly straightforward: try every possible range by considering every pair of numbers and check if it covers elements from each of the lists. This approach is inefficient as it does not leverage any particular property of the input but rather examines every possibility.

### Steps
1. Collect all numbers along with their corresponding list index.
2. Sort these numbers.
3. Iterate through every pair of numbers and check the minimum range that covers at least one number from each list.

### Code
```typescript
function smallestRange(nums: number[][]): number[] {
    let allNumbers: {value: number, listIndex: number}[] = [];
    
    // Collect all numbers with their list index
    for (let i = 0; i < nums.length; i++) {
        for (let num of nums[i]) {
            allNumbers.push({value: num, listIndex: i});
        }
    }
    
    // Sort the collected numbers
    allNumbers.sort((a, b) => a.value - b.value);
    
    // Variables for the best range found
    let minRange = [-Infinity, Infinity];
    
    // For each pair of numbers, check if it includes at least one number from each list
    for (let i = 0; i < allNumbers.length; i++) {
        let seenLists = new Set<number>();
        for (let j = i; j < allNumbers.length; j++) {
            seenLists.add(allNumbers[j].listIndex);
            if (seenLists.size === nums.length) {
                if (allNumbers[j].value - allNumbers[i].value < minRange[1] - minRange[0]) {
                    minRange = [allNumbers[i].value, allNumbers[j].value];
                }
                break;
            }
        }
    }
    
    return minRange;
}
```

### Complexity
- **Time Complexity:** O((N log N) + N^2), where N is the total number of elements across all lists. Sorting takes O(N log N) and the double loop takes O(N^2).
- **Space Complexity:** O(N), to store the combined list of numbers.

---

## Approach 2: Priority Queue (Min Heap)

### Intuition
A more efficient way to solve this problem is to use a priority queue to maintain the smallest element and expand the range by the largest current elements. This ensures that the computation is more directed towards finding a minimal range.

### Steps
1. Initialize a priority queue (min-heap) with the first element of each list and keep track of the maximum value amongst them.
2. Continuously pop the smallest element from the heap, and push the next element from the same list into the heap.
3. Update the maximum value as we push, and track the minimum range encountered that covers elements from every list.
4. Stop when we have exhausted one list as we cannot cover all lists anymore.

### Code
```typescript
import MinHeap from 'some-min-heap-library'; // Assume a MinHeap implementation

function smallestRange(nums: number[][]): number[] {
    let minHeap = new MinHeap<{value: number, row: number, col: number}>((a, b) => a.value - b.value);
    let max = -Infinity;
    let rangeStart = 0, rangeEnd = Infinity;
    
    // Initialize heap with the first element of each list
    for (let i = 0; i < nums.length; i++) {
        minHeap.insert({value: nums[i][0], row: i, col: 0});
        max = Math.max(max, nums[i][0]);
    }

    // Process the elements
    while (true) {
        let minElement = minHeap.extractMin();
        let currentRange = max - minElement.value;

        // Check if the current range is smaller
        if (currentRange < rangeEnd - rangeStart) {
            rangeStart = minElement.value;
            rangeEnd = max;
        }

        // Check to push the next element from the same list
        if (minElement.col + 1 < nums[minElement.row].length) {
            let nextCol = minElement.col + 1;
            let nextValue = nums[minElement.row][nextCol];
            minHeap.insert({value: nextValue, row: minElement.row, col: nextCol});
            max = Math.max(max, nextValue);
        } else {
            break; // We can stop if we cannot pull new elements from this list
        }
    }
    
    return [rangeStart, rangeEnd];
}
```

### Complexity
- **Time Complexity:** O(N log K), where N is the total number of elements across all lists, and K is the number of lists. Each insertion and extraction from the heap take O(log K).
- **Space Complexity:** O(K), for the heap that stores one element from each list.

This solution is optimal for this problem and efficiently finds the smallest range.

