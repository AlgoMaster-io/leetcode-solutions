[Leetcode Problem 692: Top K Frequent Words](https://leetcode.com/problems/top-k-frequent-words/)

## Approaches
- [Approach 1: Sorting and Hash Map](#approach-1-sorting-and-hash-map)
- [Approach 2: Min-Heap](#approach-2-min-heap)

---

### Approach 1: Sorting and Hash Map

**Intuition:**

The first approach uses a hash map to count the frequency of each word, then sorts the words by frequency and lexicographical order. Here's a detailed breakdown:

1. **Count Frequencies:** We use a hash map to traverse the list and count the occurrences of each word. The word is the key, and the frequency is the value.

2. **Sorting by Frequency and Alphabetically:** Once we have the frequencies, we sort the unique words by their frequency in descending order. If two words have the same frequency, we sort them by lexicographical order.

3. **Select Top K Words:** Finally, we select the top `k` frequent words from the sorted result.

**Code:**

```python
import collections

def topKFrequent(words, k):
    # Step 1: Count the frequency of each word
    count = collections.Counter(words)
    
    # Step 2: Sort the words by frequency and lexicographical order
    sorted_words = sorted(count.keys(), key=lambda word: (-count[word], word))
    
    # Step 3: Return the top k frequent words
    return sorted_words[:k]
```

**Time Complexity:** O(N log N), where N is the number of words. The log N factor comes from sorting the words.

**Space Complexity:** O(N) for storing the hash map and the sorted list.

---

### Approach 2: Min-Heap

**Intuition:**

Using a min-heap is a more optimal solution that involves the following steps:

1. **Count Frequencies:** Similar to Approach 1, we first count the frequency of each word using a hash map.

2. **Using a Min-Heap for Top K Elements:** A min-heap naturally helps with keeping track of the top `k` elements efficiently. For each word, we push the tuple `(-frequency, word)` onto the heap. The negative frequency allows us to simulate a max-heap using Python's min-heap (which is the default in Python).

3. **Ensure Correct Order in the Heap:** Python's heap compares tuples element-wise, which means it will use the first element (frequency) primarily. If there are ties, it will use the second element (word) to break ties lexicographically.

4. **Extract the Top K Words:** Once we have our heap built with the correct number of elements, we extract from the heap to get the result.

**Code:**

```python
import collections
import heapq

def topKFrequent(words, k):
    # Step 1: Count the frequency of each word
    count = collections.Counter(words)
    
    # Step 2: Create a min-heap with a maximum size of k
    heap = []
    for word, freq in count.items():
        # Step 3: Push the negative frequency to simulate max-heap
        heapq.heappush(heap, (-freq, word))
        
        # Ensure heap size does not exceed k
        if len(heap) > k:
            heapq.heappop(heap)
    
    # Step 4: Extract the words from the heap and produce the result
    result = []
    while heap:
        result.append(heapq.heappop(heap)[1])
    
    # Since the smallest frequencies are popped last, reverse the list
    result.reverse()
    return result
```

**Time Complexity:** O(N log k), where N is the total number of words and k is the number of top frequent words.

**Space Complexity:** O(N + k) due to the hash map storage and the heap, with space proportional to the number of unique words in the worst-case scenario.

