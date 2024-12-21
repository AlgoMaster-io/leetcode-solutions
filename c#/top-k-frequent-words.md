# [Leetcode 692: Top K Frequent Words](https://leetcode.com/problems/top-k-frequent-words/)

## Approaches

- [Approach 1: Naive Approach Using HashMap and List Sorting](#approach-1)
- [Approach 2: Heap-based Approach for Efficient Min-Heap with Sorted Output](#approach-2)
- [Approach 3: Bucket Sort with Frequency and Alphabetical Order](#approach-3)

### Approach 1: Naive Approach Using HashMap and List Sorting

**Intuition:**

The simplest way to solve the problem is by using a HashMap to count the frequency of each word. After populating the map, the words can be listed and sorted based on their frequencies and alphabetical order. While this approach is straightforward, it can be suboptimal in terms of time complexity because sorting can be expensive.

**Steps:**

1. Traverse the list of words, storing each word with its frequency in a HashMap.
2. Extract the keys and sort them by their frequency in descending order, and in case of a tie by their natural alphabetical order.
3. Return the top `k` words from the sorted list.

```csharp
public IList<string> TopKFrequentWords(string[] words, int k) {
    // Dictionary to store the frequency of each word
    Dictionary<string, int> frequencyMap = new Dictionary<string, int>();

    // Count the frequency of each word
    foreach (string word in words) {
        if (!frequencyMap.ContainsKey(word)) {
            frequencyMap[word] = 0;
        }
        frequencyMap[word]++;
    }

    // Get a list of keys (words) and sort them using custom comparator
    List<string> candidates = frequencyMap.Keys.ToList();
    candidates.Sort((word1, word2) => {
        int count1 = frequencyMap[word1];
        int count2 = frequencyMap[word2];
        // If frequencies are the same, sort by alphabet
        if (count1 == count2) {
            return word1.CompareTo(word2);
        }
        // Otherwise, sort by frequency
        return count2 - count1;
    });

    // Return top k words
    return candidates.Take(k).ToList();
}
```

**Complexity Analysis:**

- **Time Complexity:** O(N log N), where N is the number of unique words. Sorting the list dominates the complexity.
- **Space Complexity:** O(N), to store the frequencies in the dictionary.

### Approach 2: Heap-based Approach for Efficient Min-Heap with Sorted Output

**Intuition:**

A more optimal approach utilizes a PriorityQueue (min-heap) to maintain the top `k` frequent words efficiently. This allows us to keep the most relevant items in a smaller, manageable data structure without needing to sort all words.

**Steps:**

1. Build a frequency map similar to Approach 1.
2. Use a min-heap with a custom comparator to maintain the top `k` words. The heap property handles removal of the least frequent word when size exceeds `k`.
3. Extract the top words from the heap for the result.

```csharp
public IList<string> TopKFrequentWords(string[] words, int k) {
    // Dictionary to store the frequency of each word
    Dictionary<string, int> frequencyMap = new Dictionary<string, int>();

    // Count the frequency of each word
    foreach (string word in words) {
        if (!frequencyMap.ContainsKey(word)) {
            frequencyMap[word] = 0;
        }
        frequencyMap[word]++;
    }

    // Use a priority queue to store words by frequency
    PriorityQueue<string, WordFrequency> heap = new PriorityQueue<string, WordFrequency>();

    // Anonymous function to compare WordFrequency
    Comparison<WordFrequency> freqComparator = (x, y) => {
        if (x.Frequency != y.Frequency) return x.Frequency - y.Frequency;
        return string.Compare(y.Word, x.Word);
    };

    // Populate the heap with frequencies
    foreach (var entry in frequencyMap) {
        heap.Enqueue(entry.Key, new WordFrequency(entry.Key, entry.Value), freqComparator);
        // Keep the size of the heap to k
        if (heap.Count > k) {
            heap.Dequeue();
        }
    }

    // Extract elements from the heap into a list
    List<string> topKWords = new List<string>();
    while (heap.Count > 0) {
        topKWords.Insert(0, heap.Dequeue());
    }

    return topKWords;
}

// Helper class for Priority queue
public class WordFrequency {
    public string Word { get; }
    public int Frequency { get; }
    
    public WordFrequency(string word, int frequency) {
        Word = word;
        Frequency = frequency;
    }
}
```

**Complexity Analysis:**

- **Time Complexity:** O(N log k), as we insert elements into the heap which has maximum size `k`.
- **Space Complexity:** O(N + k), due to storage requirements for the frequency map and heap.

### Approach 3: Bucket Sort with Frequency and Alphabetical Order

**Intuition:**

Instead of sorting all the elements, we can use bucket sort to categorize words by frequency. This optimizes the time spent in comparison, especially beneficial when `k` is much smaller than the number of unique words.

**Steps:**

1. Create buckets where the index represents the frequency of occurrence.
2. Populate buckets with lists of words having the corresponding frequency.
3. Iterate over the buckets from high to low frequency to get the top `k` words. Sort words in the same bucket alphabetically.

```csharp
public IList<string> TopKFrequentWords(string[] words, int k) {
    // Dictionary to store the frequency of each word
    Dictionary<string, int> frequencyMap = new Dictionary<string, int>();

    // Count the frequency of each word
    foreach (string word in words) {
        if (!frequencyMap.ContainsKey(word)) {
            frequencyMap[word] = 0;
        }
        frequencyMap[word]++;
    }

    // Create a list to serve as buckets
    List<List<string>> buckets = new List<List<string>>(words.Length + 1);
    for (int i = 0; i <= words.Length; i++) {
        buckets.Add(new List<string>());
    }

    // Fill the buckets
    foreach (var entry in frequencyMap) {
        int frequency = entry.Value;
        buckets[frequency].Add(entry.Key);
    }

    // Iterate from the back to get the most frequent k elements
    List<string> result = new List<string>();
    for (int i = buckets.Count - 1; i >= 0 && result.Count < k; i--) {
        if (buckets[i].Count > 0) {
            buckets[i].Sort();
            result.AddRange(buckets[i].Take(k - result.Count));
        }
    }

    return result;
}
```

**Complexity Analysis:**

- **Time Complexity:** O(N), this is very efficient, assuming fixed bucket size for sorting.
- **Space Complexity:** O(N), similarly for the extra storage used for the buckets and the dictionary.

