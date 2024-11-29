# [767. Reorganize String](https://leetcode.com/problems/reorganize-string/)

## Approach 1: Max Heap

### Solution
typescript
```typescript
// Time Complexity: O(n log k), where k is the number of unique characters
// Space Complexity: O(n + k)

function reorganizeString(s: string): string {
    // Frequency map for characters
    const charCounts = new Array(26).fill(0);
    for (const c of s) {
        charCounts[c.charCodeAt(0) - 'a'.charCodeAt(0)]++;
    }

    // Priority queue (max heap) to keep the most frequent character at the top
    const maxHeap = new MaxHeap<number[]>((a, b) => b[1] - a[1]);
    for (let i = 0; i < 26; i++) {
        if (charCounts[i] > 0) {
            maxHeap.offer([i, charCounts[i]]);
        }
    }

    const result: string[] = [];
    let prev: number[] = [-1, 0]; // Previously used character and its count

    while (!maxHeap.isEmpty()) {
        const current = maxHeap.poll()!;
        result.push(String.fromCharCode(current[0] + 'a'.charCodeAt(0))); // Append current character
        current[1]--; // Decrease its count

        // Re-add the previous character if it's still valid
        if (prev[1] > 0) {
            maxHeap.offer(prev);
        }

        // Update the previous character
        prev = current;
    }

    // If the result length is not the same as the input, return ""
    return result.length === s.length ? result.join('') : '';
}

// Helper class for MaxHeap
class MaxHeap<T> {
    private heap: T[];
    private comparator: (a: T, b: T) => number;

    constructor(comparator: (a: T, b: T) => number) {
        this.heap = [];
        this.comparator = comparator;
    }

    offer(value: T): void {
        this.heap.push(value);
        this._heapifyUp();
    }

    poll(): T | undefined {
        if (this.heap.length === 0) return undefined;
        if (this.heap.length === 1) return this.heap.pop();
        const topValue = this.heap[0];
        this.heap[0] = this.heap.pop()!;
        this._heapifyDown();
        return topValue;
    }

    isEmpty(): boolean {
        return this.heap.length === 0;
    }

    private _heapifyUp(): void {
        let index = this.heap.length - 1;
        const value = this.heap[index];
        while (index > 0) {
            const parentIndex = Math.floor((index - 1) / 2);
            const parentValue = this.heap[parentIndex];
            if (this.comparator(value, parentValue) >= 0) break;
            this.heap[index] = parentValue;
            index = parentIndex;
        }
        this.heap[index] = value;
    }

    private _heapifyDown(): void {
        let index = 0;
        const length = this.heap.length;
        const value = this.heap[index];
        while (index < length) {
            const leftIndex = 2 * index + 1;
            const rightIndex = 2 * index + 2;
            let smallerChildIndex = leftIndex;
            if (rightIndex < length && this.comparator(this.heap[rightIndex], this.heap[leftIndex]) < 0) {
                smallerChildIndex = rightIndex;
            }
            if (smallerChildIndex >= length || this.comparator(this.heap[smallerChildIndex], value) >= 0) break;
            this.heap[index] = this.heap[smallerChildIndex];
            index = smallerChildIndex;
        }
        this.heap[index] = value;
    }
}
```

## Approach 2: Greedy with Frequency Array

### Solution
typescript
```typescript
// Time Complexity: O(n)
// Space Complexity: O(1) (assuming a fixed alphabet size)

function reorganizeString(s: string): string {
    const charCounts = new Array(26).fill(0);
    let maxCount = 0;
    let maxChar = '';

    // Count frequencies and find the most frequent character
    for (const c of s) {
        const index = c.charCodeAt(0) - 'a'.charCodeAt(0);
        charCounts[index]++;
        if (charCounts[index] > maxCount) {
            maxCount = charCounts[index];
            maxChar = c;
        }
    }

    // If the most frequent character cannot fit, return ""
    if (maxCount > Math.floor((s.length + 1) / 2)) {
        return '';
    }

    const result = new Array(s.length).fill('');
    let index = 0;

    // Place the most frequent character at even indices
    const maxIndex = maxChar.charCodeAt(0) - 'a'.charCodeAt(0);
    while (charCounts[maxIndex] > 0) {
        result[index] = maxChar;
        index += 2;
        charCounts[maxIndex]--;
    }

    // Place the remaining characters
    for (let i = 0; i < 26; i++) {
        while (charCounts[i] > 0) {
            if (index >= result.length) {
                index = 1; // Start filling odd indices
            }
            result[index] = String.fromCharCode(i + 'a'.charCodeAt(0));
            index += 2;
            charCounts[i]--;
        }
    }

    return result.join('');
}
```

