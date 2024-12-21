# [767. Reorganize String](https://leetcode.com/problems/reorganize-string/)

## Approaches

1. [Greedy with Max Heap](#approach-1-greedy-with-max-heap)
2. [Count and Sort](#approach-2-count-and-sort)

## Approach 1: Greedy with Max Heap

### Intuition

The main idea of this approach is to always place the most frequent character at gaps created by previously placed characters. We can achieve this by utilizing a max heap. A max heap allows us to always extract the character with the highest frequency, ensuring that we manage the placement optimally for the next character to be different from the previous one.

### Steps:

1. Count the frequency of each character in the string.
2. Use a max heap (using negative frequencies, since Python's `heapq` is a min-heap by default) to store these frequencies along with their corresponding characters.
3. Initialize a result array to construct the output string.
4. While the heap is not empty, extract two most frequent characters and place them in the result string.
5. If a character still has remaining occurrences, place it back into the heap.
6. Continue this process until you've emptied the heap.
7. Return the result as a string if it's a valid reorganized string or an empty string if it's not possible to reorganize.

### Code

```python
import heapq
from collections import Counter

def reorganizeString(S: str) -> str:
    # Step 1: Frequency count of each character
    char_count = Counter(S)
    
    # Step 2: Max heap based on frequency (negating frequency for max heap behavior)
    max_heap = [(-freq, char) for char, freq in char_count.items()]
    heapq.heapify(max_heap)
    
    # Step 3: Initialize result array
    result = []
    
    # Step 4 & 5: Extract elements from the heap and reorganize
    while len(max_heap) > 1:
        # Pop two most frequent elements
        freq1, char1 = heapq.heappop(max_heap)
        freq2, char2 = heapq.heappop(max_heap)
        
        # Append them to result
        result.extend([char1, char2])
        
        # Reduce frequency and add back to heap if still needed
        if freq1 + 1 < 0:
            heapq.heappush(max_heap, (freq1 + 1, char1))
        if freq2 + 1 < 0:
            heapq.heappush(max_heap, (freq2 + 1, char2))
    
    # Step 6: Check if there's one item left
    if max_heap:
        freq, char = heapq.heappop(max_heap)
        if freq + 1 < 0:
            return ""  # More than one occurrence left cannot place without adjacent
        result.append(char)
    
    return "".join(result)

```

### Complexity Analysis

- **Time Complexity:** O(n log k), where n is the length of the string and k is the number of unique characters. This is mainly due to heap operations.
- **Space Complexity:** O(k), where k is the number of unique characters stored in the heap.

## Approach 2: Count and Sort

### Intuition

An alternate approach relies on sorting the characters based on frequency and then attempting to interleave them in a way that no two adjacent characters are the same. While the max heap is more intuitive for this problem, counting and then sorting can also give insight.

### Steps:

1. Count the frequency of each character.
2. Sort the characters by frequency.
3. Place the most frequent character at every even index first, then fill up the odd indices.
4. If at any point filling these indices is not possible, return an empty string.

### Code

```python
from collections import Counter

def reorganizeString(S: str) -> str:
    # Step 1: Frequency count
    char_count = Counter(S)
    
    # Step 2: Sort characters by frequency
    sorted_chars = sorted(char_count, key=lambda x: -char_count[x])
    
    # Step 3: Prepare result array
    res = [''] * len(S)
    index = 0
    
    # Step 4: Fill characters starting with most frequent
    for char in sorted_chars:
        count = char_count[char]
        if count > (len(S) + 1) // 2:
            return ""  # Not possible to organize
        
        for _ in range(count):
            if index >= len(S):
                index = 1  # Move to odd index
            res[index] = char
            index += 2
    
    return ''.join(res)

```

### Complexity Analysis

- **Time Complexity:** O(n + k log k), where n is the length of the string and k is the number of unique characters. The sorting step determines the log factor.
- **Space Complexity:** O(n), due to the additional result array and the character counter.

