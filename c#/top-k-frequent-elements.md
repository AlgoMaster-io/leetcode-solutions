# [Leetcode 347: Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/)

## Approaches
1. [Basic Approach: Sorting and Dictionary](#basic-approach-sorting-and-dictionary)
2. [Optimized Approach: Min-Heap](#optimized-approach-min-heap)
3. [Most Optimal Approach: Bucket Sort](#most-optimal-approach-bucket-sort)

### Basic Approach: Sorting and Dictionary

**Intuition**:  
The problem can initially be approached by using a dictionary to count the frequency of each element. Once we have all the frequencies, we convert them into a list of key-value pairs, sort them by frequency, and then take the top K elements.

```csharp
public int[] TopKFrequent(int[] nums, int k) {
    // Use a dictionary to count the frequency of each element
    Dictionary<int, int> frequencyMap = new Dictionary<int, int>();
    foreach (int num in nums) {
        if (frequencyMap.ContainsKey(num)) {
            frequencyMap[num]++;
        } else {
            frequencyMap[num] = 1;
        }
    }

    // Convert the dictionary entries to a list and sort by frequency
    var sortedFrequency = frequencyMap.ToList();
    sortedFrequency.Sort((a, b) => b.Value.CompareTo(a.Value));

    // Select the top K elements
    int[] topKFrequent = new int[k];
    for (int i = 0; i < k; i++) {
        topKFrequent[i] = sortedFrequency[i].Key;
    }

    return topKFrequent;
}
```

- **Time Complexity**: O(N log N), where N is the number of elements in `nums`, because of sorting the frequency list.
- **Space Complexity**: O(N), for storing frequency counts and sorted list of items.

### Optimized Approach: Min-Heap

**Intuition**:  
To optimize, we can limit the number of elements we keep in consideration using a heap data structure. By making a min-heap of size K, we can efficiently keep track of the top K frequent elements seen so far, ejecting the least frequent when we find a more frequent element.

```csharp
public int[] TopKFrequent(int[] nums, int k) {
    Dictionary<int, int> frequencyMap = new Dictionary<int, int>();
    foreach (var num in nums) {
        if (frequencyMap.ContainsKey(num)) {
            frequencyMap[num]++;
        } else {
            frequencyMap[num] = 1;
        }
    }
    
    // Initialize a min-heap using SortedList (or PriorityQueue in .NET 6)
    SortedList<int, List<int>> minHeap = new SortedList<int, List<int>>();
    
    foreach (var entry in frequencyMap) {
        int freq = entry.Value;
        if (!minHeap.ContainsKey(freq)) {
            minHeap[freq] = new List<int>();
        }
        minHeap[freq].Add(entry.Key);
        
        while (minHeap.Count > k) {
            var firstKey = minHeap.Keys.First();
            minHeap[firstKey].RemoveAt(0);
            if (minHeap[firstKey].Count == 0) {
                minHeap.Remove(firstKey);
            }
        }
    }

    // Collect the top K frequent elements from the heap
    List<int> result = new List<int>();
    foreach (var entry in minHeap) {
        result.AddRange(entry.Value);
    }

    return result.ToArray();
}
```

- **Time Complexity**: O(N log K), since each of the N elements is inserted into the heap of size K.
- **Space Complexity**: O(N + K), for frequency map storage and the min-heap.

### Most Optimal Approach: Bucket Sort

**Intuition**:  
We can take advantage of the fact that there are a limited number of frequencies (at max N), which allows us to use a bucket sort technique. Instead of sorting the elements directly, we sort the frequencies using an array of lists (buckets), where the index represents the frequency.

```csharp
public int[] TopKFrequent(int[] nums, int k) {
    Dictionary<int, int> frequencyMap = new Dictionary<int, int>();
    foreach (int num in nums) {
        if (frequencyMap.ContainsKey(num)) {
            frequencyMap[num]++;
        } else {
            frequencyMap[num] = 1;
        }
    }

    // Create buckets where index represents frequency
    List<int>[] buckets = new List<int>[nums.Length + 1];
    foreach (var pair in frequencyMap) {
        int frequency = pair.Value;
        if (buckets[frequency] == null) {
            buckets[frequency] = new List<int>();
        }
        buckets[frequency].Add(pair.Key);
    }

    // Collect top K frequent elements from the buckets
    List<int> result = new List<int>();
    for (int i = buckets.Length - 1; i >= 0 && result.Count < k; i--) {
        if (buckets[i] != null) {
            result.AddRange(buckets[i]);
        }
    }

    return result.ToArray();
}
```

- **Time Complexity**: O(N), because we are essentially going over the list of elements twice, once for counting and the other for collecting frequencies.
- **Space Complexity**: O(N), for storing frequencies and bucket list.

This approach is not only efficient but also intuitive once you understand the bucketing technique, especially suitable when the range of frequency values is limited by the size of input.

