# [LeetCode Problem 347: Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/)

## Approaches

- [Approach 1: Sorting](#approach-1-sorting)
- [Approach 2: Heap (Min-Heap)](#approach-2-heap-min-heap)
- [Approach 3: Bucket Sort](#approach-3-bucket-sort)

## Approach 1: Sorting

### Intuition

The intuition behind this approach is to first count the frequency of each element in the array, and then retrieve the `k` elements with the highest frequencies by sorting them.

### Steps

1. **Frequency Map**: Use a hash map to count occurrences of each element.
2. **Sort Elements by Frequency**: Convert the frequency map to an array of key-value pairs and sort it by frequency in descending order.
3. **Retrieve the Top K Elements**: Select the first `k` entries from the sorted array.

### Code

```typescript
function topKFrequent(nums: number[], k: number): number[] {
    const frequencyMap = new Map<number, number>();

    // Count the frequency of each element
    for (const num of nums) {
        frequencyMap.set(num, (frequencyMap.get(num) || 0) + 1);
    }

    // Sort elements by frequency in descending order
    const sortedFrequency = Array.from(frequencyMap.entries()).sort((a, b) => b[1] - a[1]);

    // Extract the top k elements
    return sortedFrequency.slice(0, k).map(entry => entry[0]);
}
```

### Time and Space Complexity

- **Time Complexity**: O(N log N), where N is the number of elements. This is because sorting takes O(N log N).
- **Space Complexity**: O(N), due to storing the frequency map.

## Approach 2: Heap (Min-Heap)

### Intuition

A more optimal approach for this problem is using a Min-Heap of size `k` to keep track of the top `k` elements. This approach is more efficient when `k` is smaller compared to the number of unique elements.

### Steps

1. **Frequency Map**: Count the frequency of each element.
2. **Min-Heap**: Use a Min-Heap of size `k` to keep the top `k` frequent elements.
3. **Maintain the Min-Heap**: For each unique element, if its frequency is larger than the smallest frequency in the heap, replace it.

### Code

```typescript
function topKFrequent(nums: number[], k: number): number[] {
    const frequencyMap = new Map<number, number>();

    // Count the frequency of each element
    for (const num of nums) {
        frequencyMap.set(num, (frequencyMap.get(num) || 0) + 1);
    }

    // Min-Heap to track the top k frequent elements
    const minHeap = new MinPriorityQueue<{ key: number; priority: number }>({
        priority: (x) => x.priority,
    });

    for (const [num, freq] of frequencyMap.entries()) {
        minHeap.enqueue({ key: num, priority: freq });
        
        // Maintain the size of the heap to k
        if (minHeap.size() > k) {
            minHeap.dequeue();
        }
    }

    // Extract the elements from the heap
    const result: number[] = [];
    while (!minHeap.isEmpty()) {
        result.push(minHeap.dequeue().element.key);
    }

    return result;
}
```

### Time and Space Complexity

- **Time Complexity**: O(N log k), where N is the number of elements. Maintaining a heap of size `k` takes O(log k) time.
- **Space Complexity**: O(N + k), for the frequency map and the heap.

## Approach 3: Bucket Sort

### Intuition

Bucket sort leverages the fact that the maximum frequency of an element in an array can't be more than the number of elements in the array. We use this insight to create buckets indexed by frequency and distribute the elements into these buckets.

### Steps

1. **Frequency Map**: Count the frequency of each element.
2. **Buckets**: Create an array of empty lists, where index corresponds to frequency.
3. **Fill Buckets**: Place each element in the corresponding bucket based on its frequency.
4. **Collect Results**: Iterate over the buckets from the back (highest frequency) and collect elements until we have `k` elements.

### Code

```typescript
function topKFrequent(nums: number[], k: number): number[] {
    const frequencyMap = new Map<number, number>();

    // Count the frequency of each element
    for (const num of nums) {
        frequencyMap.set(num, (frequencyMap.get(num) || 0) + 1);
    }

    const buckets: number[][] = Array.from({ length: nums.length + 1 }, () => []);

    // Fill the buckets
    for (const [num, freq] of frequencyMap.entries()) {
        buckets[freq].push(num);
    }

    // Collect the top k frequent elements
    const result: number[] = [];
    for (let i = buckets.length - 1; i > 0 && result.length < k; i--) {
        if (buckets[i].length > 0) {
            result.push(...buckets[i]);
        }
    }

    return result.slice(0, k);
}
```

### Time and Space Complexity

- **Time Complexity**: O(N), where N is the number of elements. Counting frequencies and distributing them into buckets are linear operations.
- **Space Complexity**: O(N), due to frequency map and buckets.

