# LeetCode Problem 632: Smallest Range Covering Elements from K Lists

[LeetCode 632: Smallest Range Covering Elements from K Lists](https://leetcode.com/problems/smallest-range-covering-elements-from-k-lists/)

## Approaches

1. [Brute Force](#brute-force)
2. [Two-Pointer with Heap](#two-pointer-with-heap)

### Brute Force

In the brute force approach, we aim to consider every possible range of numbers that includes at least one number from each of the lists. While this approach is not optimal due to its time complexity, it helps us understand the problem statement more deeply.

#### Intuition

1. The basic idea is to generate all possible pairs of (min, max) by examining each number.
2. For each pair (min, max), ensure it covers at least one number from every list.
3. If it does, check if it is the smallest of such ranges found so far.
4. Return the smallest valid range.

#### Code

```javascript
var smallestRange = function(nums) {
    let minRange = [-100000, 100000]; // Start with the largest possible range
    for (let i = 0; i < nums.length; i++) {
        for (let j = 0; j < nums[i].length; j++) {
            const start = nums[i][j];
            let maxElement = start;
            let minElement = start;
            let validRange = true;
            for (let k = 0; k < nums.length; k++) {
                if (!nums[k].some(x => x >= start)) {
                    validRange = false;
                    break;
                }
                const filtered = nums[k].filter(x => x >= start);
                maxElement = Math.max(maxElement, Math.min(...filtered));
            }

            if (validRange && maxElement - minElement < minRange[1] - minRange[0]) {
                minRange = [minElement, maxElement];
            }
        }
    }
    return minRange;
};
```

#### Complexity Analysis

- **Time Complexity:** O(N^2 * M), where N is the number of lists, and M is the average number of elements in the lists. 
- **Space Complexity:** O(1) for extra space.

### Two-Pointer with Heap

For a more optimal solution, we can use a min-heap (priority queue) along with a two-pointer technique. This is a more advanced approach taking advantage of sorting and priority queue features to efficiently find the smallest range.

#### Intuition

1. Each time we add the smallest current element from the min-heap, which tells us the current minimum value in the range.
2. Track the largest number encountered. This helps us define the maximum of the current range.
3. Adjust the pointers by removing the smallest number from the current range (popped from heap), then move to the next number in that list.
4. Update the heap and recalculate the range using the new smallest and current maximum value.
5. Continue the process until we can't cover all lists.
6. Since we're using sorted lists and a heap, the range can be efficiently reduced.

#### Code

```javascript
var smallestRange = function(nums) {
    const heap = new MinPriorityQueue({priority: (element) => element.val});
    let max = -Infinity;
    let rangeStart = 0;
    let rangeEnd = Infinity;

    // Initialize heap with the first elements of each list
    for (let i = 0; i < nums.length; i++) {
        heap.enqueue({val: nums[i][0], listIdx: i, eleIdx: 0});
        max = Math.max(max, nums[i][0]);
    }

    while (heap.size() === nums.length) {
        const {val: min, listIdx, eleIdx} = heap.dequeue();

        // Update the minimum range if the current range is smaller
        if (max - min < rangeEnd - rangeStart) {
            rangeStart = min;
            rangeEnd = max;
        }

        // Move to the next element in the list which had the minimum element
        if (eleIdx + 1 < nums[listIdx].length) {
            const next = {val: nums[listIdx][eleIdx + 1], listIdx: listIdx, eleIdx: eleIdx + 1};
            heap.enqueue(next);
            max = Math.max(max, next.val);
        }
    }

    return [rangeStart, rangeEnd];
};
```

#### Complexity Analysis

- **Time Complexity:** O(N * M * log N), as each step involves log N operation due to heap insertions. N is the number of lists and M the length of each list (considering they are of similar size for average case).
- **Space Complexity:** O(N) for the heap storage.

