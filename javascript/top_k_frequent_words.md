# [692. Top K Frequent Words](https://leetcode.com/problems/top-k-frequent-words/)

## Approach 1: HashMap and Min-Heap

### Solution
```javascript
// Time Complexity: O(n log k)
// Space Complexity: O(n)
function topKFrequent(words, k) {
    // Count the frequency of each word
    const frequencyMap = new Map();
    for (const word of words) {
        frequencyMap.set(word, (frequencyMap.get(word) || 0) + 1);
    }

    // Use a min-heap to keep the top k frequent words
    const minHeap = new MinHeap((a, b) => 
        frequencyMap.get(a) === frequencyMap.get(b) 
            ? b.localeCompare(a) // Sort lexicographically for equal frequency
            : frequencyMap.get(a) - frequencyMap.get(b)
    );

    for (const word of frequencyMap.keys()) {
        minHeap.offer(word);
        if (minHeap.size() > k) {
            minHeap.poll();
        }
    }

    // Build the result list from the heap
    const result = [];
    while (!minHeap.isEmpty()) {
        result.push(minHeap.poll());
    }
    return result.reverse(); // Reverse to get the most frequent words first
}

// MinHeap utility class for managing the heap operations
class MinHeap {
    constructor(comparator) {
        this.data = [];
        this.comparator = comparator;
    }

    offer(val) {
        this.data.push(val);
        this.bubbleUp();
    }

    poll() {
        if (this.size() === 0) return null;
        const top = this.data[0];
        const bottom = this.data.pop();
        if (this.size() > 0) {
            this.data[0] = bottom;
            this.bubbleDown();
        }
        return top;
    }

    size() {
        return this.data.length;
    }

    isEmpty() {
        return this.size() === 0;
    }

    bubbleUp() {
        let index = this.data.length - 1;
        while (index > 0) {
            const parentIndex = (index - 1) >>> 1;
            if (this.comparator(this.data[parentIndex], this.data[index]) <= 0) break;
            [this.data[parentIndex], this.data[index]] = [this.data[index], this.data[parentIndex]];
            index = parentIndex;
        }
    }

    bubbleDown() {
        let index = 0;
        const length = this.data.length;
        const element = this.data[0];

        while (true) {
            let leftChildIndex = 2 * index + 1;
            let rightChildIndex = 2 * index + 2;
            let swapIndex = null;

            if (leftChildIndex < length) {
                if (this.comparator(this.data[leftChildIndex], element) < 0) {
                    swapIndex = leftChildIndex;
                }
            }

            if (rightChildIndex < length) {
                if (
                    (swapIndex === null && this.comparator(this.data[rightChildIndex], element) < 0) || 
                    (swapIndex !== null && this.comparator(this.data[rightChildIndex], this.data[leftChildIndex]) < 0)
                ) {
                    swapIndex = rightChildIndex;
                }
            }

            if (swapIndex === null) break;

            [this.data[index], this.data[swapIndex]] = [this.data[swapIndex], element];
            index = swapIndex;
        }
    }
}
```

## Approach 2: HashMap and Sorting

### Solution
```javascript
// Time Complexity: O(n + k log k)
// Space Complexity: O(n)
function topKFrequent(words, k) {
    // Count the frequency of each word
    const frequencyMap = new Map();
    for (const word of words) {
        frequencyMap.set(word, (frequencyMap.get(word) || 0) + 1);
    }

    // Sort the words by frequency and lexicographical order
    const candidates = Array.from(frequencyMap.keys());
    candidates.sort((a, b) => 
        frequencyMap.get(a) === frequencyMap.get(b) 
            ? a.localeCompare(b) // Lexicographical order for equal frequency
            : frequencyMap.get(b) - frequencyMap.get(a) // Higher frequency first
    );

    // Return the top k words
    return candidates.slice(0, k);
}
```

