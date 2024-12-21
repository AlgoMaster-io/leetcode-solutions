# [Leetcode 451: Sort Characters By Frequency](https://leetcode.com/problems/sort-characters-by-frequency/)

## Approaches
- [Approach 1: Brute-force sorting](#approach-1-brute-force-sorting)
- [Approach 2: Bucket Sort](#approach-2-bucket-sort)
- [Approach 3: Using Priority Queue (Heap) (Optimal)](#approach-3-using-priority-queue-heap-optimal)

---

## Approach 1: Brute-force sorting

### Intuition
The simplest way to solve the problem is to count the frequency of each character and then sort the characters based on their frequency. This can be achieved using a hashmap to count frequencies and then converting it to a list or array for sorting.

### Steps:
1. Create a hashmap to store the frequency of each character.
2. Convert the hashmap to a list of characters.
3. Sort the list using a custom comparator based on frequency.
4. Build the resulting string based on sorted order.

### Implementation
```java
import java.util.*;

public class Solution {
    public String frequencySort(String s) {
        // Step 1: Count frequency of each character
        Map<Character, Integer> frequencyMap = new HashMap<>();
        for (char c : s.toCharArray()) {
            frequencyMap.put(c, frequencyMap.getOrDefault(c, 0) + 1);
        }
        
        // Step 2: Convert the map to a list of characters
        List<Character> characters = new ArrayList<>(frequencyMap.keySet());
        
        // Step 3: Sort the list by frequency
        characters.sort((a, b) -> frequencyMap.get(b) - frequencyMap.get(a));
        
        // Step 4: Build the final string
        StringBuilder sb = new StringBuilder();
        for (char c : characters) {
            int freq = frequencyMap.get(c);
            for (int i = 0; i < freq; i++) {
                sb.append(c);
            }
        }
        
        return sb.toString();
    }
}
```

### Time Complexity
O(n log n) - due to sorting the characters.

### Space Complexity
O(n) - for storing the frequency map and the result.

---

## Approach 2: Bucket Sort

### Intuition
To improve on the sorting time, we can use bucket sort which exploits the fact that frequency maximum is bounded by the string length. By placing characters into buckets indexed by frequency, we can directly build the string.

### Steps:
1. Count frequency of each character and find the maximum frequency.
2. Create buckets where each index corresponds to characters that have that frequency.
3. Fill the buckets with the corresponding characters.
4. Traverse the buckets in reverse order to create the output string by frequency.

### Implementation
```java
import java.util.*;

public class Solution {
    public String frequencySort(String s) {
        // Step 1: Count frequency of each character
        Map<Character, Integer> frequencyMap = new HashMap<>();
        for (char c : s.toCharArray()) {
            frequencyMap.put(c, frequencyMap.getOrDefault(c, 0) + 1);
        }

        // Step 2: Create buckets for frequencies
        int maxFrequency = s.length();
        List<Character>[] buckets = new List[maxFrequency + 1];
        for (char c : frequencyMap.keySet()) {
            int frequency = frequencyMap.get(c);
            if (buckets[frequency] == null) {
                buckets[frequency] = new ArrayList<>();
            }
            buckets[frequency].add(c);
        }

        // Step 3: Build the result by traversing the buckets in reverse order
        StringBuilder sb = new StringBuilder();
        for (int i = maxFrequency; i > 0; i--) {
            if (buckets[i] != null) {
                for (char c : buckets[i]) {
                    for (int j = 0; j < i; j++) {
                        sb.append(c);
                    }
                }
            }
        }

        return sb.toString();
    }
}
```

### Time Complexity
O(n) - counting the frequency and creating string, assuming bounded frequency.

### Space Complexity
O(n) - for storing the buckets and resulting string.

---

## Approach 3: Using Priority Queue (Heap) (Optimal)

### Intuition
We can also use a max-heap to always extract the most frequent element. This provides an efficient way to manage and prioritize elements by frequency without fully sorting the list of characters.

### Steps:
1. Count frequency of each character.
2. Use a priority queue (max-heap) to store entries sorted by frequency.
3. Extract characters from the priority queue and append to the result string.

### Implementation
```java
import java.util.*;

public class Solution {
    public String frequencySort(String s) {
        // Step 1: Count frequency of each character
        Map<Character, Integer> frequencyMap = new HashMap<>();
        for (char c : s.toCharArray()) {
            frequencyMap.put(c, frequencyMap.getOrDefault(c, 0) + 1);
        }

        // Step 2: Create a max-heap based on frequency
        PriorityQueue<Character> maxHeap = new PriorityQueue<>((a, b) -> frequencyMap.get(b) - frequencyMap.get(a));
        maxHeap.addAll(frequencyMap.keySet());

        // Step 3: Build the resulting string using the heap
        StringBuilder sb = new StringBuilder();
        while (!maxHeap.isEmpty()) {
            char c = maxHeap.poll();
            int count = frequencyMap.get(c);
            for (int i = 0; i < count; i++) {
                sb.append(c);
            }
        }

        return sb.toString();
    }
}
```

### Time Complexity
O(n log k) - where n is the number of characters and k is the unique characters (k ≤ n).

### Space Complexity
O(n) - for the max-heap and the result string.

---

Each solution takes advantage of different data structures and provides a balance between time complexity and space complexity. The choice of solution can depend on specific constraints and performance requirements.

