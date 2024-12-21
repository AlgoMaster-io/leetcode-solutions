# [Leetcode 347: Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/)

## Table of Contents
- [Approach 1: Brute Force with Frequency Map and Sorting](#approach-1)
- [Approach 2: Heap (Priority Queue) for Optimal Solution](#approach-2)
- [Approach 3: Bucket Sort for Optimal Solution in Time Complexity](#approach-3)

---

## Approach 1: Brute Force with Frequency Map and Sorting <a name="approach-1"></a>

### Intuition
The most straightforward approach to solving this problem is to count the frequency of each element and then sort the elements based on their frequency count to find the top `k` elements.

### Steps:
1. **Frequency Map:** Create a frequency map using a dictionary where the key is an element from the list and the value is its frequency.
2. **Sorting by Frequency:** Convert the frequency map to a list of tuples and sort it based on frequency in descending order.
3. **Extract Top `k` Elements:** Extract the first `k` elements from the sorted list as these are the top `k` frequent elements.

### Code
```python
def topKFrequent(nums, k):
    # Step 1: Build the frequency map
    frequency_map = {}
    for num in nums:
        if num in frequency_map:
            frequency_map[num] += 1
        else:
            frequency_map[num] = 1
    
    # Step 2: Sort based on frequency
    sorted_items = sorted(frequency_map.items(), key=lambda item: item[1], reverse=True)
    
    # Step 3: Get top k elements
    top_k_elements = [item[0] for item in sorted_items[:k]]
    return top_k_elements
```

### Complexity Analysis
- **Time Complexity:** O(n log n) due to sorting the frequency map.
- **Space Complexity:** O(n) for storing the frequency map.

---

## Approach 2: Heap (Priority Queue) for Optimal Solution <a name="approach-2"></a>

### Intuition
Instead of sorting the whole frequency map, we can use a min-heap to keep track of the top `k` elements efficiently. The Python `heapq` library can help maintain a heap of the top `k` frequencies.

### Steps:
1. **Frequency Map:** Similar to the brute force approach, create a frequency map.
2. **Min-Heap:** Use a min-heap of size `k` to maintain the top `k` elements. Insert each element into the heap, if the size exceeds `k`, remove the smallest frequency element.
3. **Extract Elements:** Extract the elements from the heap, which are the top `k` frequent elements.

### Code
```python
import heapq

def topKFrequent(nums, k):
    # Step 1: Build the frequency map
    frequency_map = {}
    for num in nums:
        if num in frequency_map:
            frequency_map[num] += 1
        else:
            frequency_map[num] = 1

    # Step 2: Use a heap to get top k elements
    min_heap = []
    for num, freq in frequency_map.items():
        heapq.heappush(min_heap, (freq, num))
        if len(min_heap) > k:
            heapq.heappop(min_heap)
    
    # Step 3: Extract the elements
    top_k_elements = [item[1] for item in min_heap]
    return top_k_elements
```

### Complexity Analysis
- **Time Complexity:** O(n log k), as the heap operations are logarithmic somewhat limited to size `k`.
- **Space Complexity:** O(n) to store the frequency map and O(k) for the heap.

---

## Approach 3: Bucket Sort for Optimal Solution in Time Complexity <a name="approach-3"></a>

### Intuition
An alternative approach to using heaps is to use the bucket sort principle. Since the frequency of elements is bounded by the length of the array, we can optimize further by sorting based on frequency.

### Steps:
1. **Frequency Map:** Count the frequency of each element.
2. **Bucket Array:** Create an array of lists where index represents the frequency and each list contains elements with that frequency.
3. **Populate Buckets:** Fill the relevant buckets based on frequency.
4. **Extract Top `k` Frequents:** Start from the highest frequency and collect elements until `k` elements are gathered.

### Code
```python
def topKFrequent(nums, k):
    # Step 1: Build frequency map
    frequency_map = {}
    for num in nums:
        frequency_map[num] = frequency_map.get(num, 0) + 1
    
    # Step 2: Create a list of empty lists for bucket sorting
    bucket = [[] for _ in range(len(nums) + 1)]
    
    # Step 3: Fill the buckets
    for num, freq in frequency_map.items():
        bucket[freq].append(num)
    
    # Step 4: Extract the top k frequent elements
    top_k_elements = []
    # Traverse the bucket list in reverse order
    for i in range(len(bucket) - 1, 0, -1):
        for num in bucket[i]:
            top_k_elements.append(num)
            if len(top_k_elements) == k:
                return top_k_elements
```

### Complexity Analysis
- **Time Complexity:** O(n) since we are essentially doing counting sort.
- **Space Complexity:** O(n) to store the frequency map and the bucket array.

