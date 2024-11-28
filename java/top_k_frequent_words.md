# [692. Top K Frequent Words](https://leetcode.com/problems/top-k-frequent-words/)

## Approach 1: HashMap and Min-Heap

### Solution
```java
// Time Complexity: O(n log k)
// Space Complexity: O(n)
import java.util.*;

public class Solution {
    public List<String> topKFrequent(String[] words, int k) {
        // Count the frequency of each word
        Map<String, Integer> frequencyMap = new HashMap<>();
        for (String word : words) {
            frequencyMap.put(word, frequencyMap.getOrDefault(word, 0) + 1);
        }

        // Use a min-heap to keep the top k frequent words
        PriorityQueue<String> minHeap = new PriorityQueue<>((a, b) -> 
            frequencyMap.get(a).equals(frequencyMap.get(b)) 
                ? b.compareTo(a) // Sort lexicographically for equal frequency
                : frequencyMap.get(a) - frequencyMap.get(b)
        );

        for (String word : frequencyMap.keySet()) {
            minHeap.offer(word);
            if (minHeap.size() > k) {
                minHeap.poll();
            }
        }

        // Build the result list from the heap
        List<String> result = new ArrayList<>();
        while (!minHeap.isEmpty()) {
            result.add(minHeap.poll());
        }
        Collections.reverse(result); // Reverse to get the most frequent words first
        return result;
    }
}
```

## Approach 2: HashMap and Sorting

### Solution
```java
// Time Complexity: O(n + k log k)
// Space Complexity: O(n)
import java.util.*;

public class Solution {
    public List<String> topKFrequent(String[] words, int k) {
        // Count the frequency of each word
        Map<String, Integer> frequencyMap = new HashMap<>();
        for (String word : words) {
            frequencyMap.put(word, frequencyMap.getOrDefault(word, 0) + 1);
        }

        // Sort the words by frequency and lexicographical order
        List<String> candidates = new ArrayList<>(frequencyMap.keySet());
        Collections.sort(candidates, (a, b) -> 
            frequencyMap.get(a).equals(frequencyMap.get(b)) 
                ? a.compareTo(b) // Lexicographical order for equal frequency
                : frequencyMap.get(b) - frequencyMap.get(a) // Higher frequency first
        );

        // Return the top k words
        return candidates.subList(0, k);
    }
}
```