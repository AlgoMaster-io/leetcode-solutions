# [Leetcode 692: Top K Frequent Words](https://leetcode.com/problems/top-k-frequent-words/)

## Approaches:
1. [Naive Approach using HashMap and Sorting](#naive-approach)
2. [Priority Queue (Heap) Approach](#priority-queue-approach)

---

### 1. Naive Approach using HashMap and Sorting

**Intuition:**

In this approach, we will first count the frequency of each word using a hashmap. Once we have the frequencies, we can sort the words based on the frequency and lexicographical order. Finally, we will return the `k` most frequent words.

**Steps:**
- Store the frequency of each word in a hashmap.
- Convert the hashmap keys (words) into an array and sort it based on the frequency (descending order) and lexicographical order.
- Return the first `k` words from the sorted list.

**Complexity:**
- **Time Complexity:** O(N log N), due to sorting the array of words, where N is the number of unique words.
- **Space Complexity:** O(N), for storing the frequency map and list of unique words.

```typescript
function topKFrequent(words: string[], k: number): string[] {
    // Step 1: Create a map to store the frequency of each word
    const frequencyMap = new Map<string, number>();

    // Count frequencies
    for (const word of words) {
        frequencyMap.set(word, (frequencyMap.get(word) || 0) + 1);
    }

    // Step 2: Convert map keys to an array and sort them
    const sortedWords = Array.from(frequencyMap.keys()).sort((a, b) => {
        // Sort by frequency: higher frequency comes first
        if (frequencyMap.get(a)! > frequencyMap.get(b)!) {
            return -1;
        }
        if (frequencyMap.get(a)! < frequencyMap.get(b)!) {
            return 1;
        }
        // Sort by lexicographical order if frequencies are the same
        return a.localeCompare(b);
    });

    // Step 3: Return the first k elements
    return sortedWords.slice(0, k);
}
```

---

### 2. Priority Queue (Heap) Approach

**Intuition:**

This approach is more efficient for finding the top `k` elements. Instead of sorting the entire list of words, we can maintain a min-heap to keep track of the top `k` frequent words. As we process each word, we adjust our heap accordingly.

**Steps:**
- Create a hashmap to count the frequency of each word.
- Use a min-heap (priority queue) to keep track of the top `k` words based on frequency and lexicographical order. This heap will maintain the smallest element at the top.
- For each word, add it to the heap. If the size of the heap exceeds `k`, remove the minimum element.
- Extract elements from the heap to get the top `k` frequent words.

**Complexity:**
- **Time Complexity:** O(N log k), for building the heap, where N is the number of unique words.
- **Space Complexity:** O(N + k), for frequency map and the heap.

```typescript
function topKFrequentHeap(words: string[], k: number): string[] {
    // Step 1: Count the frequency of each word using a map
    const frequencyMap = new Map<string, number>();
    
    for (const word of words) {
        frequencyMap.set(word, (frequencyMap.get(word) || 0) + 1);
    }

    // Step 2: Use a min-heap to keep the top k frequent words
    const minHeap: [string, number][] = [];

    const addWordToHeap = (word: string, frequency: number) => {
        // Add word with its frequency to the min-heap
        minHeap.push([word, frequency]);
        // Maintain the heap property
        minHeap.sort((a, b) => {
            // First by frequency (higher priority for higher frequency)
            if (a[1] !== b[1]) {
                return a[1] - b[1];
            }
            // Second by lexicographical order (higher priority for smaller lexicographical order)
            return b[0].localeCompare(a[0]);
        });
        // If heap size exceeds k, remove the least frequent word (root of the heap)
        if (minHeap.length > k) {
            minHeap.shift();
        }
    };

    // Insert all words into the heap
    for (const [word, frequency] of frequencyMap.entries()) {
        addWordToHeap(word, frequency);
    }

    // Collect results in reverse order (since it's a min-heap)
    const result: string[] = [];
    while (minHeap.length > 0) {
        result.unshift(minHeap.shift()![0]);
    }

    return result;
}
```

---

These solutions demonstrate different approaches to tackle the problem of finding the top `k` frequent words. The second approach using a heap is more efficient for larger values of `k`.

