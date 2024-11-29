# [692. Top K Frequent Words](https://leetcode.com/problems/top-k-frequent-words/)

## Approach 1: HashMap and Min-Heap

### Solution
c#
```csharp
// Time Complexity: O(n log k)
// Space Complexity: O(n)
using System;
using System.Collections.Generic;

public class Solution {
    public IList<string> TopKFrequent(string[] words, int k) {
        // Count the frequency of each word
        var frequencyMap = new Dictionary<string, int>();
        foreach (string word in words) {
            if (!frequencyMap.ContainsKey(word)) {
                frequencyMap[word] = 0;
            }
            frequencyMap[word]++;
        }

        // Use a min-heap to keep the top k frequent words
        var minHeap = new SortedSet<(int frequency, string word)>();
        foreach (var entry in frequencyMap) {
            minHeap.Add((entry.Value, entry.Key));
            if (minHeap.Count > k) {
                minHeap.Remove(minHeap.Min);
            }
        }

        // Build the result list from the heap
        var result = new List<string>();
        foreach (var entry in minHeap) {
            result.Add(entry.word);
        }
        result.Reverse(); // Reverse to get the most frequent words first
        return result;
    }
}
```

## Approach 2: HashMap and Sorting

### Solution
c#
```csharp
// Time Complexity: O(n + k log k)
// Space Complexity: O(n)
using System;
using System.Collections.Generic;

public class Solution {
    public IList<string> TopKFrequent(string[] words, int k) {
        // Count the frequency of each word
        var frequencyMap = new Dictionary<string, int>();
        foreach (string word in words) {
            if (!frequencyMap.ContainsKey(word)) {
                frequencyMap[word] = 0;
            }
            frequencyMap[word]++;
        }

        // Sort the words by frequency and lexicographical order
        var candidates = new List<string>(frequencyMap.Keys);
        candidates.Sort((a, b) => 
            frequencyMap[a] == frequencyMap[b] 
                ? string.Compare(a, b) // Lexicographical order for equal frequency
                : frequencyMap[b] - frequencyMap[a] // Higher frequency first
        );

        // Return the top k words
        return candidates.GetRange(0, k);
    }
}
```

