# [451. Sort Characters By Frequency](https://leetcode.com/problems/sort-characters-by-frequency/)

## Navigation
- [Approach 1: Frequency Counting and Sorting](#approach-1-frequency-counting-and-sorting)
- [Approach 2: Bucket Sort](#approach-2-bucket-sort)
- [Approach 3: Using Max-Heap](#approach-3-using-max-heap)

## Approach 1: Frequency Counting and Sorting

### Intuition
The first approach is relatively straightforward. The idea is to count the frequency of each character in the string and then sort these characters based on their frequency. This method involves converting the string into a frequency map, sorting the map by frequency, and then reconstructing the string.

### Steps:
1. Count the frequency of each character using a dictionary.
2. Convert the dictionary into a list of tuples where each tuple contains a character and its frequency.
3. Sort the list of tuples in descending order based on frequency.
4. Construct the resulting string by multiplying each character by its frequency and concatenating them.

### Code
```python
def frequencySort(s: str) -> str:
    # Step 1: Count the frequency of each character.
    frequency_map = {}
    for char in s:
        if char in frequency_map:
            frequency_map[char] += 1
        else:
            frequency_map[char] = 1
    
    # Step 2: Convert dictionary to a list of tuples and sort it by frequency.
    sorted_frequencies = sorted(frequency_map.items(), key=lambda item: item[1], reverse=True)
    
    # Step 3: Build the result string using sorted frequencies.
    result = []
    for char, freq in sorted_frequencies:
        result.append(char * freq)
    
    return ''.join(result)
```

### Time Complexity
- Sorting the list of characters based on frequency takes \(O(n \log n)\).
- Therefore, the overall time complexity is \(O(n \log n)\).

### Space Complexity
- We are using additional data structures like a dictionary and a list which take \(O(n)\) space.


## Approach 2: Bucket Sort

### Intuition
Since the range of frequencies is limited (at most the length of the string), we can use a bucket sort approach. We allocate an array of buckets where each bucket index corresponds to a frequency and each bucket contains characters with that frequency. 

### Steps:
1. Count the frequency of each character.
2. Create a list of empty lists (buckets), where the index represents the frequency.
3. Put each character into the appropriate bucket based on its frequency.
4. Iterate over the buckets in reverse order to construct the result string: higher frequency characters come first.

### Code
```python
def frequencySort(s: str) -> str:
    # Step 1: Count frequency of each character.
    frequency_map = {}
    for char in s:
        frequency_map[char] = frequency_map.get(char, 0) + 1
    
    # Step 2: Create buckets.
    max_freq = max(frequency_map.values(), default=0)
    buckets = [[] for _ in range(max_freq + 1)]
    
    # Step 3: Fill the buckets.
    for char, freq in frequency_map.items():
        buckets[freq].append(char)
    
    # Step 4: Construct the result string in reverse order of frequencies.
    result = []
    for freq in range(len(buckets) - 1, 0, -1):
        for char in buckets[freq]:
            result.append(char * freq)
    
    return ''.join(result)
```

### Time Complexity
- The bucketing process takes \(O(n)\), and constructing the result string also takes \(O(n)\).
- Therefore, the overall time complexity is \(O(n)\).

### Space Complexity
- The space complexity for the buckets also takes \(O(n)\).

## Approach 3: Using Max-Heap

### Intuition
Here we can efficiently get the element with the highest frequency by using a max-heap. By pushing characters with their frequency into a max-heap, we can always extract the characters with the highest frequency efficiently.

### Steps:
1. Count the frequency of each character using a dictionary.
2. Use a max-heap to store pairs of (-frequency, character). We use negative frequency because Python's heapq is a min-heap by default.
3. Construct the resulting string by repeatedly popping from the heap, making sure each character appears k times, where k is its frequency.

### Code
```python
import heapq

def frequencySort(s: str) -> str:
    # Step 1: Count frequency of each character.
    frequency_map = {}
    for char in s:
        frequency_map[char] = frequency_map.get(char, 0) + 1
    
    # Step 2: Create a max-heap using negative frequencies.
    max_heap = [(-freq, char) for char, freq in frequency_map.items()]
    heapq.heapify(max_heap)
    
    # Step 3: Build the result string using the heap.
    result = []
    while max_heap:
        freq, char = heapq.heappop(max_heap)
        result.append(char * -freq)  # Multiply character by its frequency (negative because of min-heap)
    
    return ''.join(result)
```

### Time Complexity
- The time complexity to build the heap is \(O(n \log m)\) where \(n\) is the number of characters in the string and \(m\) is the number of unique characters.
- Extracting elements until the heap is empty also takes \(O(n \log m)\).
- Therefore, the overall time complexity is \(O(n \log m)\).

### Space Complexity
- The space complexity is \(O(n)\) for storing frequency counts and results.

