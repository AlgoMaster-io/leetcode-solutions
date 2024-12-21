# [Leetcode 295: Find Median from Data Stream](https://leetcode.com/problems/find-median-from-data-stream/)

## Approaches
- [Initial Approach: Sorting After Each Insertion](#initial-approach-sorting-after-each-insertion)
- [Optimized Approach: Using Two Heaps](#optimized-approach-using-two-heaps)

### Initial Approach: Sorting After Each Insertion

#### Intuition
The simplest way to find the median is by maintaining a sorted list of all numbers. After inserting a new number, sort the list and directly access the median. This approach is straightforward but inefficient for large datasets since sorting is costly.

#### Implementation

```typescript
class MedianFinder {
    private numbers: number[];

    constructor() {
        this.numbers = [];
    }

    addNum(num: number): void {
        // Add the number to the list
        this.numbers.push(num);
        // Sort the list to maintain ordering, time complexity: O(n log n)
        this.numbers.sort((a, b) => a - b);
    }

    findMedian(): number {
        const n = this.numbers.length;
        // If even number of elements, return the average of the middle two
        if (n % 2 === 0) {
            return (this.numbers[n / 2 - 1] + this.numbers[n / 2]) / 2;
        } else { // If odd, return the middle element
            return this.numbers[Math.floor(n / 2)];
        }
    }
}
```

#### Complexity
- **Time Complexity**: O(n log n) for `addNum` due to sorting, O(1) for `findMedian`.
- **Space Complexity**: O(n) to store the numbers.

### Optimized Approach: Using Two Heaps

#### Intuition
To optimize and avoid sorting with each insertion, use two heaps:
- A max-heap (implemented as a min-heap with negated values) for the lower half of the numbers.
- A min-heap for the upper half of the numbers.

This allows inserting a number in log(n) time, and retrieving the median in constant time. We maintain that each heap can only differ in size by at most one element, and the max of the lower half is always less than or equal to the min of the upper half.

#### Implementation

```typescript
class MedianFinder {
    private lowerHalf: number[];
    private upperHalf: number[];

    constructor() {
        this.lowerHalf = []; // Max-heap (as inverted min-heap)
        this.upperHalf = []; // Min-heap
    }

    addNum(num: number): void {
        // Insert into max-heap (lowerHalf)
        this.lowerHalf.push(-num);
        this.lowerHalf.sort((a, b) => a - b);

        // Move the maximum element of lowerHalf to upperHalf
        if (this.upperHalf.length > 0 && -this.lowerHalf[0] > this.upperHalf[0]) {
            const maxLower = -this.lowerHalf.shift()!;
            this.upperHalf.push(maxLower);
            this.upperHalf.sort((a, b) => a - b);
        }

        // Balance the two heaps so that their size difference is at most 1
        if (this.lowerHalf.length > this.upperHalf.length + 1) {
            this.upperHalf.push(-this.lowerHalf.shift()!);
            this.upperHalf.sort((a, b) => a - b);
        }
        if (this.upperHalf.length > this.lowerHalf.length) {
            this.lowerHalf.push(-this.upperHalf.shift()!);
            this.lowerHalf.sort((a, b) => a - b);
        }
    }

    findMedian(): number {
        // If the number of elements is odd, median is at the top of lowerHalf
        if (this.lowerHalf.length > this.upperHalf.length) {
            return -this.lowerHalf[0];
        }
        // If even, median is the average of the tops of lowerHalf and upperHalf
        return (-this.lowerHalf[0] + this.upperHalf[0]) / 2;
    }
}
```

#### Complexity
- **Time Complexity**: O(log n) for `addNum` due to inserting into heaps, O(1) for `findMedian`.
- **Space Complexity**: O(n) to store the numbers in heaps.

