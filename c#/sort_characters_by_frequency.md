# 451. [Sort Characters By Frequency](https://leetcode.com/problems/sort-characters-by-frequency/)

## Approach 1: Using Dictionary and Sorting

### Solution
```csharp
// Time Complexity: O(n log n)
// Space Complexity: O(n)
using System;
using System.Collections.Generic;
using System.Text;

public class Solution {
    public string FrequencySort(string s) {
        // Count the frequency of each character
        Dictionary<char, int> frequencyMap = new Dictionary<char, int>();
        foreach (char c in s) {
            if (frequencyMap.ContainsKey(c)) {
                frequencyMap[c]++;
            } else {
                frequencyMap[c] = 1;
            }
        }

        // Sort characters by frequency in descending order
        PriorityQueue<KeyValuePair<char, int>, int> maxHeap = new PriorityQueue<KeyValuePair<char, int>, int>(
            Comparer<int>.Create((a, b) => b - a)
        );
        
        foreach (var entry in frequencyMap) {
            maxHeap.Enqueue(entry, entry.Value);
        }

        // Build the result string
        StringBuilder result = new StringBuilder();
        while (maxHeap.Count > 0) {
            var entry = maxHeap.Dequeue();
            for (int i = 0; i < entry.Value; i++) {
                result.Append(entry.Key);
            }
        }

        return result.ToString();
    }
}
```

## Approach 2: Bucket Sort (Optimized for Large Inputs)

### Solution
```csharp
// Time Complexity: O(n)
// Space Complexity: O(n)
using System;
using System.Collections.Generic;
using System.Text;

public class Solution {
    public string FrequencySort(string s) {
        // Count the frequency of each character
        int[] frequency = new int[128]; // Assuming ASCII characters
        foreach (char c in s) {
            frequency[c]++;
        }

        // Create buckets where index represents frequency
        List<char>[] buckets = new List<char>[s.Length + 1];
        for (int i = 0; i < frequency.Length; i++) {
            if (frequency[i] > 0) {
                if (buckets[frequency[i]] == null) {
                    buckets[frequency[i]] = new List<char>();
                }
                buckets[frequency[i]].Add((char)i);
            }
        }

        // Build the result string from the buckets
        StringBuilder result = new StringBuilder();
        for (int i = buckets.Length - 1; i > 0; i--) {
            if (buckets[i] != null) {
                foreach (char c in buckets[i]) {
                    for (int j = 0; j < i; j++) {
                        result.Append(c);
                    }
                }
            }
        }

        return result.ToString();
    }
}
```

