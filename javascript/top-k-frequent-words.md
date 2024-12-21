# [692. Top K Frequent Words](https://leetcode.com/problems/top-k-frequent-words/)

## Solutions

- [Approach 1: Brute Force with Sorting](#approach-1-brute-force-with-sorting)
- [Approach 2: Using a Hash Map and Min-Heap](#approach-2-using-a-hash-map-and-min-heap)


### Approach 1: Brute Force with Sorting

#### Intuition

The simplest way to solve this problem is by counting the frequency of each word, sorting by their frequency, and returning the top `k` frequent words. Sorting would be based firstly on frequency and then lexicographically for words having the same frequency.

#### Steps

1. Count the frequency of each word using a hash map.
2. Convert the map to a list of tuples.
3. Sort the list:
   - Primarily by frequency in descending order.
   - Secondary by word in ascending order (for ties).
4. Return the top `k` elements from the sorted list.

```javascript
function topKFrequent(words, k) {
    // Step 1: Count frequencies using a hash map
    const frequencyMap = {};
    for (let word of words) {
        frequencyMap[word] = (frequencyMap[word] || 0) + 1;
    }

    // Step 2 & 3: Sort the words by frequency and lexicographical order
    const sortedWords = Object.keys(frequencyMap).sort(
        (a, b) => frequencyMap[b] - frequencyMap[a] || a.localeCompare(b)
    );

    // Step 4: Return top k frequent words
    return sortedWords.slice(0, k);
}
```

#### Complexity Analysis

- **Time Complexity:** O(n log n), where n is the total number of unique words. Sorting dominates this approach.
- **Space Complexity:** O(n), where n is the number of unique words stored in the hash map.

### Approach 2: Using a Hash Map and Min-Heap

#### Intuition

To improve our sorting step, we can use a min-heap to efficiently retrieve the top `k` frequent elements. By using a priority queue (min-heap of size `k`), we can maintain the most frequent elements efficiently.

#### Steps

1. Count the frequency of each word using a hash map.
2. Use a min-heap to store the top `k` frequent elements. This ensures that we efficiently push new elements and pop the least frequent elements out when the heap exceeds size `k`.
3. Order by frequency in descending order and lexicographically in ascending order using a custom comparator.

```javascript
function topKFrequent(words, k) {
    // Step 1: Count frequencies using a hash map
    const frequencyMap = {};
    for (let word of words) {
        frequencyMap[word] = (frequencyMap[word] || 0) + 1;
    }

    // Helper function to maintain a min-heap
    function minHeapCompare(a, b) {
        return frequencyMap[a] === frequencyMap[b]
            ? b.localeCompare(a) // for same frequency, higher lexicographical string will be prioritized
            : frequencyMap[a] - frequencyMap[b]; // for different frequency, lower frequency will be prioritized
    }

    const minHeap = [];
    const insertHeapWithMaxSize = function (word) {
        minHeap.push(word);
        minHeap.sort(minHeapCompare); // Not the most efficient way to maintain a min-heap, but simple to illustrate
        if (minHeap.length > k) {
            minHeap.shift(); // Remove the least frequent element when exceeding size k
        }
    };

    // Insert elements into the heap
    for (let word in frequencyMap) {
        insertHeapWithMaxSize(word);
    }

    // As this is a min-heap, the elements are in ascending order
    minHeap.sort(
        (a, b) => frequencyMap[b] - frequencyMap[a] || a.localeCompare(b)
    );

    return minHeap;
}
```

#### Complexity Analysis

- **Time Complexity:** O(n log k), where n is the number of unique elements. The heap maintains the top k elements efficiently, having log k insertions.
- **Space Complexity:** O(n + k), where n is for the hash map of all unique words, and k for the min-heap.

