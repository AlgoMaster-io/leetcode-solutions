# [Leetcode 767: Reorganize String](https://leetcode.com/problems/reorganize-string/)

## Solutions
- [Approach 1: Brute Force with Permutations](#approach-1-brute-force-with-permutations)
- [Approach 2: Greedy Approach with Priority Queue](#approach-2-greedy-approach-with-priority-queue)

### Approach 1: Brute Force with Permutations

#### Intuition

The straightforward way to solve this problem is to generate all possible permutations of the string and check each one to see if it meets the requirement that no two adjacent characters are the same. This is a classic brute force method which is generally inefficient for large inputs due to its high complexity.

#### Code

```csharp
public class Solution {
    public string ReorganizeString(string s) {
        // Check if a valid permutation exists
        // For example if a character appears more than (N+1)/2 times, it's impossible to reorganize
        int[] count = new int[26];
        int maxFreq = 0;
        int n = s.Length;
        
        foreach (char c in s) {
            count[c - 'a']++;
            maxFreq = Math.Max(maxFreq, count[c - 'a']);
        }
        
        if (maxFreq > (n + 1) / 2) return "";

        // All permutations of string s
        char[] chars = s.ToCharArray();
        Array.Sort(chars);
        do {
            if (IsValid(chars)) {
                return new string(chars);
            }
        } while (NextPermutation(chars));
        
        return "";
    }
    
    private bool IsValid(char[] chars){
        for (int i = 1; i < chars.Length; ++i) {
            if (chars[i] == chars[i - 1]) {
                return false;
            }
        }
        return true;
    }

    private bool NextPermutation(char[] array) {
        int i = array.Length - 2;
        while (i >= 0 && array[i] >= array[i + 1]) i--;
        if (i < 0) return false;
        
        int j = array.Length - 1;
        while (array[j] <= array[i]) j--;
        
        Swap(array, i, j);
        Array.Reverse(array, i + 1, array.Length - i - 1);
        return true;
    }

    private void Swap(char[] array, int i, int j) {
        char tmp = array[i];
        array[i] = array[j];
        array[j] = tmp;
    }
}
```

#### Complexity Analysis

- **Time Complexity**: O(n!), where n is the length of the string.
- **Space Complexity**: O(n), for storing the string and permutation results.

### Approach 2: Greedy Approach with Priority Queue

#### Intuition

The idea is to use a priority queue (or max heap) to always get the character with the largest remaining frequency. We can then form a string by placing these characters in a greedy manner such that we avoid placing the same character adjacent to each other.

Steps:
- Count the frequency of each character.
- Use a priority queue to maintain characters by their frequency in descending order.
- Repeatedly select the two most frequent characters and add them in alternating pattern until all characters are used up.

#### Code

```csharp
public class Solution {
    public string ReorganizeString(string s) {
        // Frequency dictionary
        int[] frequency = new int[26];
        int n = s.Length;
        foreach (char c in s) {
            frequency[c - 'a']++;
        }

        // Priority Queue, max heap based on frequency
        PriorityQueue<(char, int), int> pq = new PriorityQueue<(char, int), int>(Comparer<int>.Create((a, b) => b - a));
        for (int i = 0; i < 26; i++) {
            if (frequency[i] > 0) {
                // If any character frequency is more than half the string length, return empty
                if (frequency[i] > (n + 1) / 2) {
                    return "";
                }
                pq.Enqueue(((char)(i + 'a'), frequency[i]), frequency[i]);
            }
        }

        // Reorganize the string
        StringBuilder result = new StringBuilder();
        while (pq.Count > 1) {
            // Get two most frequent letters
            var (firstChar, firstFreq) = pq.Dequeue();
            var (secondChar, secondFreq) = pq.Dequeue();

            // Add them to the result
            result.Append(firstChar);
            result.Append(secondChar);

            // Decrement frequency and possibly add back to the queue
            if (--firstFreq > 0) {
                pq.Enqueue((firstChar, firstFreq), firstFreq);
            }
            if (--secondFreq > 0) {
                pq.Enqueue((secondChar, secondFreq), secondFreq);
            }
        }

        // Handle the last character, if any
        if (pq.Count > 0) {
            result.Append(pq.Dequeue().Item1);
        }

        return result.ToString();
    }
}
```

#### Complexity Analysis

- **Time Complexity**: O(n log k), where n is the length of the string and k is the number of distinct characters. The most time-consuming operation here is inserting into a priority queue.
- **Space Complexity**: O(n), needed for the result storage and auxiliary data structures.

