# Leetcode Problem: [692. Top K Frequent Words](https://leetcode.com/problems/top-k-frequent-words/)

## Approaches
- [Approach 1: Frequency Map with Sorting](#approach-1-frequency-map-with-sorting)
- [Approach 2: Min-Heap](#approach-2-min-heap)

---

## Approach 1: Frequency Map with Sorting
### Intuition
The main idea is to count the frequency of each word and then sort them by frequency. Since we need the top k frequent words, we can sort the entries and select the first k elements. As words with identical frequencies should be in alphabetical order, we ensure that in our sort function.

### Steps
1. Use a hashmap to count the frequency of each word.
2. Convert the hashmap into a list of entries (word and frequency pairs).
3. Sort the list with a custom comparator:
   - First, by decreasing frequency.
   - Second, lexicographically in case of a tie in frequency.
4. Extract the first k elements from the sorted list.

### Time Complexity
- O(N log N), where N is the number of unique words. Sorting the list of words dictates the complexity.

### Space Complexity
- O(N), where N is the number of unique words (storing in hashmap and list).

```java
import java.util.*;

public List<String> topKFrequent(String[] words, int k) {
    // Step 1: Frequency mapping
    Map<String, Integer> count = new HashMap<>();
    for (String word : words) {
        count.put(word, count.getOrDefault(word, 0) + 1);
    }
    
    // Step 2 and 3: Convert hashmap to list and sort
    List<String> candidates = new ArrayList<>(count.keySet());
    Collections.sort(candidates, (w1, w2) -> {
        // First sort by frequency (decreasing order)
        if (count.get(w1).equals(count.get(w2))) {
            // Then sort lexicographically in case of tie
            return w1.compareTo(w2);
        } else {
            return count.get(w2) - count.get(w1);
        }
    });
    
    // Step 4: Extract the top k elements
    return candidates.subList(0, k);
}
```

---

## Approach 2: Min-Heap
### Intuition
A min-heap can efficiently help maintain the k most frequent elements, especially when we need to sort by frequency primarily. The idea is to always maintain k elements in the heap and eject elements when the heap grows beyond k, ensuring that the heap contains the most frequent words.

### Steps
1. Count the frequency of each word using a hashmap.
2. Use a PriorityQueue (min-heap) to maintain the k highest frequency words.
   - When adding new elements to the heap of size more than k, remove the smallest frequency word.
   - Use a custom comparator to sort by frequency and lexicographical order for words with the same frequency.
3. Extract elements from the heap to form the result list.

### Time Complexity
- O(N log k), as we perform heap operations (insertions/deletions) proportional to the number of unique words, where each operation takes O(log k).

### Space Complexity
- O(N), for storing the hashmap and the heap potentially containing all unique words.

```java
import java.util.*;

public List<String> topKFrequent(String[] words, int k) {
    // Step 1: Frequency mapping
    Map<String, Integer> count = new HashMap<>();
    for (String word : words) {
        count.put(word, count.getOrDefault(word, 0) + 1);
    }
    
    // Step 2: Min-Heap to store top k frequent words
    PriorityQueue<String> heap = new PriorityQueue<>(
        (w1, w2) -> count.get(w1).equals(count.get(w2)) 
                    ? w2.compareTo(w1) 
                    : count.get(w1) - count.get(w2));
    
    for (String word : count.keySet()) {
        heap.offer(word);
        if (heap.size() > k) {
            heap.poll(); // Remove the word with the smallest frequency
        }
    }
    
    // Step 3: Form result list from the heap
    List<String> result = new ArrayList<>();
    while (!heap.isEmpty()) {
        result.add(heap.poll());
    }
    Collections.reverse(result); // Since we want the largest frequencies first
    return result;
}
```

By utilizing these approaches, you can efficiently determine the top k frequent words in a given list using java.

