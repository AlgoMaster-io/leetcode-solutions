# [Leetcode 347: Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/)

## Approaches

- [Approach 1: Frequency Map and Sorting](#approach-1-frequency-map-and-sorting)
- [Approach 2: Heap (Priority Queue)](#approach-2-heap-priority-queue)
- [Approach 3: Bucket Sort](#approach-3-bucket-sort)

### Approach 1: Frequency Map and Sorting

**Intuition:**

The simplest way to solve this problem is to count the frequency of each element using a hash map, then sort the elements based on their frequency in descending order, and finally, take the top k elements.

**Steps:**
1. Use a hash map to record the frequency of each number from the input array `nums`.
2. Convert the hash map's keys (the elements) to an array.
3. Sort the keys based on their frequency in descending order.
4. Extract the first `k` elements from the sorted list to return as the result.

**Time Complexity:** O(N log N) — Sorting the elements by their frequency dominates the complexity.

**Space Complexity:** O(N) — Storing frequencies in a hash map.

```javascript
function topKFrequent(nums, k) {
    let frequencyMap = new Map();

    // Build the frequency map
    for (let num of nums) {
        frequencyMap.set(num, (frequencyMap.get(num) || 0) + 1);
    }

    // Convert map keys to array and sort based on frequency
    let elements = Array.from(frequencyMap.keys());
    elements.sort((a, b) => frequencyMap.get(b) - frequencyMap.get(a));

    // Get the top k elements
    return elements.slice(0, k);
}
```

### Approach 2: Heap (Priority Queue)

**Intuition:**

To improve upon the sorting solution, we can use a min-heap to keep track of the top k elements by frequency. The heap allows us to efficiently retrieve the top k frequent elements without needing to sort the entire list.

**Steps:**
1. Count each number's frequency using a hash map.
2. Create a min-heap where elements are sorted based on frequency.
3. Iterate over the hash map, and for each element, push it onto the heap but maintain only the k most frequent elements in the heap.
4. If the heap size exceeds k, remove the least frequent element from the heap.
5. The heap now contains the k most frequent elements.

**Time Complexity:** O(N log K) — Pushing to and popping from the heap both take O(log K), and each is done for N elements.

**Space Complexity:** O(N) — For the frequency map.

```javascript
function topKFrequent(nums, k) {
    let frequencyMap = new Map();

    // Build the frequency map
    for (let num of nums) {
        frequencyMap.set(num, (frequencyMap.get(num) || 0) + 1);
    }

    let minHeap = new MinPriorityQueue({ priority: x => x[1] });

    // Maintain a heap for the top k frequent elements
    for (let [num, freq] of frequencyMap.entries()) {
        minHeap.enqueue([num, freq]);

        if (minHeap.size() > k) {
            minHeap.dequeue();
        }
    }

    // Extract the elements from the heap
    let topK = [];
    while (minHeap.size() > 0) {
        topK.push(minHeap.dequeue().element[0]);
    }
    return topK;
}
```

### Approach 3: Bucket Sort

**Intuition:**

Newly discovered optimization leverages bucket sort principle. By assuming maximum possible frequency is `N` (when every number is the same), we can use an array where the index represents the frequency, and at each index we store a list of numbers that have that frequency.

**Steps:**
1. Count each number's frequency using a hash map.
2. Create an array of buckets, where index represents frequency.
3. Populate the buckets with numbers from the hash map based on their frequency.
4. Start from the highest frequency bucket, traverse through the buckets and collect the top k frequent elements.

**Time Complexity:** O(N) — Buckets handle in linear time by bypassing full sort.

**Space Complexity:** O(N) — Array storage for numbers & buckets.

```javascript
function topKFrequent(nums, k) {
    let frequencyMap = new Map();

    // Build the frequency map
    for (let num of nums) {
        frequencyMap.set(num, (frequencyMap.get(num) || 0) + 1);
    }

    // Initialize the bucket array
    let bucket = Array.from({ length: nums.length + 1 }, () => []);

    // Fill the buckets
    for (let [num, freq] of frequencyMap.entries()) {
        bucket[freq].push(num);
    }

    // Collect the top k frequent elements
    let topK = [];
    for (let i = bucket.length - 1; i >= 0 && topK.length < k; i--) {
        if (bucket[i].length > 0) {
            topK.push(...bucket[i]);
        }
    }

    return topK.slice(0, k);
}
```

The bucket sort approach is very efficient for this problem, boasting a time complexity that is linear relative to the number of elements, making it ideal for large size inputs.

