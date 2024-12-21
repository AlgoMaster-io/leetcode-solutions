# [Leetcode 767: Reorganize String](https://leetcode.com/problems/reorganize-string/)

## Approaches:
- [Approach 1: Greedy with Frequency Count](#approach-1-greedy-with-frequency-count)
- [Approach 2: Greedy with Max Heap](#approach-2-greedy-with-max-heap)

### Approach 1: Greedy with Frequency Count

#### Intuition:
The main idea is to count the frequency of each character and determine if it's possible to reorganize the string such that no two adjacent characters are the same. The key insight here is that if the most frequent character count is greater than half of the string length plus one (for odd lengths), it's impossible to achieve. Otherwise, we can fill even indices with the most frequent character first and then fill the rest.

#### Steps:
1. Count the frequency of each character.
2. Check if the maximum frequency is greater than `(n + 1) / 2`. If it is, return an empty string because it's impossible to reorganize.
3. Place the most frequent character first at every even index.
4. Fill in the less frequent characters afterward.

```typescript
function reorganizeString(s: string): string {
    const frequency: Map<string, number> = new Map();
    for (const char of s) {
        frequency.set(char, (frequency.get(char) || 0) + 1);
    }

    const maxFrequency = Math.max(...frequency.values());
    if (maxFrequency > Math.ceil(s.length / 2)) {
        return ""; // impossible to reorganize
    }

    const result: string[] = new Array(s.length);
    const sortedChars = Array.from(frequency.keys()).sort((a, b) => frequency.get(b)! - frequency.get(a)!);

    let index = 0;
    for (const char of sortedChars) {
        let count = frequency.get(char)!;
        while (count > 0) {
            result[index] = char;
            index += 2;
            if (index >= s.length) {
                index = 1; // move to the odd indices
            }
            count--;
        }
    }

    return result.join('');
}
```

- **Time Complexity:** O(n), where n is the length of the string, because counting character frequencies and filling the resultant array are both linear operations.
- **Space Complexity:** O(n) for storing the resultant array, frequency map, and sorted characters.

### Approach 2: Greedy with Max Heap

#### Intuition:
This approach uses a max heap to always pick the character with the highest frequency that isn't immediately repeatable. The idea is to extract the top two frequent characters from the heap, place them in the result, and reinsert them with updated frequencies. By always picking the two most frequent available characters, we ensure that the solution remains valid.

#### Steps:
1. Count the frequency of each character.
2. Push all characters with their frequencies as a tuple into a max heap (using a priority queue).
3. Extract two top elements (most frequent characters), place them in the result while decreasing their count.
4. Re-push them back into the heap if they still have a positive frequency.
5. Continue this until the heap is empty.

```typescript
class MaxHeap {
    private heap: [string, number][];
    
    constructor() {
        this.heap = [];
    }
    
    insert(char: string, freq: number) {
        this.heap.push([char, freq]);
        this.heapifyUp();
    }
    
    extractMax(): [string, number] | null {
        if (this.heap.length === 0) return null;
        if (this.heap.length === 1) return this.heap.pop()!;
        
        const max = this.heap[0];
        this.heap[0] = this.heap.pop()!;
        this.heapifyDown();
        return max;
    }
    
    private heapifyUp() {
        let index = this.heap.length - 1;
        while (index > 0) {
            const parentIndex = Math.floor((index - 1) / 2);
            if (this.heap[parentIndex][1] >= this.heap[index][1]) break;
            [this.heap[parentIndex], this.heap[index]] = [this.heap[index], this.heap[parentIndex]];
            index = parentIndex;
        }
    }
    
    private heapifyDown() {
        let index = 0;
        while (index < this.heap.length) {
            const leftChild = 2 * index + 1;
            const rightChild = 2 * index + 2;
            let largest = index;
            
            if (leftChild < this.heap.length && this.heap[leftChild][1] > this.heap[largest][1]) {
                largest = leftChild;
            }
            
            if (rightChild < this.heap.length && this.heap[rightChild][1] > this.heap[largest][1]) {
                largest = rightChild;
            }
            
            if (largest !== index) {
                [this.heap[largest], this.heap[index]] = [this.heap[index], this.heap[largest]];
                index = largest;
            } else {
                break;
            }
        }
    }
    
    isEmpty(): boolean {
        return this.heap.length === 0;
    }
}

function reorganizeString(s: string): string {
    const frequency: Map<string, number> = new Map();
    for (const char of s) {
        frequency.set(char, (frequency.get(char) || 0) + 1);
    }
    
    const maxHeap = new MaxHeap();
    for (const [char, freq] of frequency.entries()) {
        maxHeap.insert(char, freq);
    }

    const result: string[] = [];
    while (!maxHeap.isEmpty()) {
        const [char1, freq1] = maxHeap.extractMax()!;
        result.push(char1);
        
        if (!maxHeap.isEmpty()) {
            const [char2, freq2] = maxHeap.extractMax()!;
            result.push(char2);
            
            if (freq2 > 1) maxHeap.insert(char2, freq2 - 1);
        }
        
        if (freq1 > 1) maxHeap.insert(char1, freq1 - 1);
    }
    
    return result.length === s.length ? result.join('') : "";
}
```

- **Time Complexity:** O(n log k), where n is the length of the string and k is the number of unique characters, due to heap operations.
- **Space Complexity:** O(n) for storing the frequency map and resultant string.

