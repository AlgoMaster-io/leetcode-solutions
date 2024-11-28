# [451. Sort Characters By Frequency](https://leetcode.com/problems/sort-characters-by-frequency/)

## Approach 1: Using HashMap and Sorting

### Solution
```java
// Time Complexity: O(n log n)
// Space Complexity: O(n)
import java.util.HashMap;
import java.util.Map;
import java.util.PriorityQueue;

public class Solution {
    public String frequencySort(String s) {
        // Count the frequency of each character
        Map<Character, Integer> frequencyMap = new HashMap<>();
        for (char c : s.toCharArray()) {
            frequencyMap.put(c, frequencyMap.getOrDefault(c, 0) + 1);
        }

        // Sort characters by frequency in descending order
        PriorityQueue<Map.Entry<Character, Integer>> maxHeap = new PriorityQueue<>(
            (a, b) -> b.getValue() - a.getValue()
        );
        maxHeap.addAll(frequencyMap.entrySet());

        // Build the result string
        StringBuilder result = new StringBuilder();
        while (!maxHeap.isEmpty()) {
            Map.Entry<Character, Integer> entry = maxHeap.poll();
            for (int i = 0; i < entry.getValue(); i++) {
                result.append(entry.getKey());
            }
        }

        return result.toString();
    }
}
```

## Approach 2: Bucket Sort (Optimized for Large Inputs)

### Solution
```java
// Time Complexity: O(n)
// Space Complexity: O(n)
import java.util.ArrayList;
import java.util.List;

public class Solution {
    public String frequencySort(String s) {
        // Count the frequency of each character
        int[] frequency = new int[128]; // Assuming ASCII characters
        for (char c : s.toCharArray()) {
            frequency[c]++;
        }

        // Create buckets where index represents frequency
        List<Character>[] buckets = new List[s.length() + 1];
        for (int i = 0; i < frequency.length; i++) {
            if (frequency[i] > 0) {
                if (buckets[frequency[i]] == null) {
                    buckets[frequency[i]] = new ArrayList<>();
                }
                buckets[frequency[i]].add((char) i);
            }
        }

        // Build the result string from the buckets
        StringBuilder result = new StringBuilder();
        for (int i = buckets.length - 1; i > 0; i--) {
            if (buckets[i] != null) {
                for (char c : buckets[i]) {
                    for (int j = 0; j < i; j++) {
                        result.append(c);
                    }
                }
            }
        }

        return result.toString();
    }
}
```