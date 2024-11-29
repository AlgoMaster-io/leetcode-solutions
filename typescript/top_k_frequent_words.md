# [692. Top K Frequent Words](https://leetcode.com/problems/top-k-frequent-words/)

## Approach 1: HashMap and Min-Heap

### Solution

typescript
```typescript
// Time Complexity: O(n log k)
// Space Complexity: O(n)
function topKFrequent(words: string[], k: number): string[] {
    const frequencyMap: Map<string, number> = new Map();

    // Count the frequency of each word
    words.forEach(word => {
        frequencyMap.set(word, (frequencyMap.get(word) || 0) + 1);
    });

    // Use a min-heap to keep the top k frequent words
    const minHeap: string[] = [];
    [...frequencyMap.keys()].forEach(word => {
        minHeap.push(word);
        minHeap.sort((a, b) =>
            frequencyMap.get(a) === frequencyMap.get(b)
                ? b.localeCompare(a) // Sort lexicographically for equal frequency
                : (frequencyMap.get(a) as number) - (frequencyMap.get(b) as number)
        );

        if (minHeap.length > k) {
            minHeap.shift();
        }
    });

    // Build the result list from the heap
    minHeap.sort((a, b) =>
        frequencyMap.get(a) === frequencyMap.get(b)
            ? a.localeCompare(b) // Lexicographical order for equal frequency
            : (frequencyMap.get(b) as number) - (frequencyMap.get(a) as number)
    );

    return minHeap.slice(0, k);
}
```

## Approach 2: HashMap and Sorting

### Solution

typescript
```typescript
// Time Complexity: O(n + k log k)
// Space Complexity: O(n)
function topKFrequent(words: string[], k: number): string[] {
    const frequencyMap: Map<string, number> = new Map();

    // Count the frequency of each word
    words.forEach(word => {
        frequencyMap.set(word, (frequencyMap.get(word) || 0) + 1);
    });

    // Sort the words by frequency and lexicographical order
    const candidates: string[] = [...frequencyMap.keys()];
    candidates.sort((a, b) =>
        frequencyMap.get(a) === frequencyMap.get(b)
            ? a.localeCompare(b) // Lexicographical order for equal frequency
            : (frequencyMap.get(b) as number) - (frequencyMap.get(a) as number) // Higher frequency first
    );

    // Return the top k words
    return candidates.slice(0, k);
}
```

