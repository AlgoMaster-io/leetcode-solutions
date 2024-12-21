# [Leetcode 767: Reorganize String](https://leetcode.com/problems/reorganize-string/)

## Approaches:
- [Approach 1: Greedy Approach with Counting](#approach-1-greedy-approach-with-counting)
- [Approach 2: Priority Queue (Heap)](#approach-2-priority-queue-heap)

---
## Approach 1: Greedy Approach with Counting

### Intuition:
The most basic approach to solve this problem is to count the frequency of each character. We place the most frequent element at even indices to maximize the chance for any subsequent character to be placed in between without repetition.

### Solution Steps:
1. **Count the Frequencies**:
   - Use an array to count occurrences of each character.
   - The size of the array will be 26 if the string only contains lowercase English letters.

2. **Check if Reorganization is Possible**:
   - Find the character with the maximum count.
   - Check if its frequency is more than (n+1)/2 where n is the length of the given string. If yes, it's impossible to reorganize the string.

3. **Fill Characters Alternately**:
   - Start filling the result in such a way that no two same characters are adjacent.
   - Start with even positions for the maximum frequency character, then fill odd positions.

### Complexity:
- **Time Complexity**: O(n), where n is the length of the string.
- **Space Complexity**: O(1), because we are using constant extra space to store frequencies.

```java
public String reorganizeString(String S) {
    int[] count = new int[26];
    for (char c : S.toCharArray()) {
        count[c - 'a']++;
    }

    // Find the character with the maximum frequency
    int maxCount = 0;
    char maxChar = 'a';
    for (int i = 0; i < 26; i++) {
        if (count[i] > maxCount) {
            maxCount = count[i];
            maxChar = (char) (i + 'a');
        }
    }

    // Check if reorganization is possible
    if (maxCount > (S.length() + 1) / 2) {
        return "";
    }

    char[] result = new char[S.length()];
    int index = 0;

    // Place the highest frequency character at even positions
    while (count[maxChar - 'a'] > 0) {
        result[index] = maxChar;
        index += 2;
        count[maxChar - 'a']--;
    }

    // Fill other characters
    for (int i = 0; i < 26; i++) {
        while (count[i] > 0) {
            if (index >= result.length) {
                index = 1;  // Switch to odd positions
            }
            result[index] = (char) (i + 'a');
            index += 2;
            count[i]--;
        }
    }
    
    return new String(result);
}
```

---
## Approach 2: Priority Queue (Heap)

### Intuition:
The optimal way to distribute characters without adjacency is to use a max heap which allows us to always pick the character with the highest frequency available. The idea is to append characters based on their frequency in a greedy manner.

### Solution Steps:
1. **Count Frequencies**:
   - Use a frequency map to count each character.

2. **Use Max-Heap**:
   - Build a max heap (priority queue) based on frequencies of characters.
   - This enables us to always extract the most frequent character efficiently.

3. **Construct String**:
   - Continuously extract the two most frequent characters and append them to the result string.
   - Decrease their count and reinsert them into the heap if they still have a pending count.
   - If at the end, a single character remains with positive count, return an empty string because it's not possible to reorganize.

### Complexity:
- **Time Complexity**: O(n log k), where n is the size of the input string and k is the number of distinct characters.
- **Space Complexity**: O(k), to store characters in the heap.

```java
import java.util.PriorityQueue;
import java.util.Map;
import java.util.HashMap;

public String reorganizeString(String S) {
    Map<Character, Integer> countMap = new HashMap<>();
    for (char c : S.toCharArray()) {
        countMap.put(c, countMap.getOrDefault(c, 0) + 1);
    }

    PriorityQueue<Character> maxHeap = new PriorityQueue<>((a, b) -> countMap.get(b) - countMap.get(a));
    maxHeap.addAll(countMap.keySet());

    StringBuilder result = new StringBuilder();

    while (maxHeap.size() > 1) {
        char first = maxHeap.poll();
        char second = maxHeap.poll();

        result.append(first);
        result.append(second);

        countMap.put(first, countMap.get(first) - 1);
        countMap.put(second, countMap.get(second) - 1);

        if (countMap.get(first) > 0) {
            maxHeap.offer(first);
        }
        if (countMap.get(second) > 0) {
            maxHeap.offer(second);
        }
    }

    if (!maxHeap.isEmpty()) {
        char last = maxHeap.poll();
        if (countMap.get(last) > 1) {
            return "";
        }
        result.append(last);
    }

    return result.toString();
}
```

In summary, both approaches ensure that no adjacent characters are the same; however, the optimal way when considering different string sizes and character distributions is the priority queue method.

