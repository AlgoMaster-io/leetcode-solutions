# [528. Random Pick with Weight](https://leetcode.com/problems/random-pick-with-weight/)

## Approaches:
- [Prefix Sum + Binary Search Approach](#prefix-sum--binary-search-approach)
- [Binary Search Tree Approach (Optimal)](#binary-search-tree-approach-optimal)

---

### Prefix Sum + Binary Search Approach

**Intuition:**

The main goal is to pick an index `i` with a probability proportional to `w[i]`. If we imagine each weight as a segment on a line of length `sum(weights)`, then we can pick a random point on this line and determine which segment it falls into using binary search. Thus, the problem boils down to constructing a prefix sum array and using binary search to determine our pick.

1. Compute prefix sums of the weights. This helps in cumulatively understanding the "length" of each segment.
2. Use random function to pick a target in the range of 0 to the total sum of weights.
3. Use binary search to find the index where the target falls in the prefix sum array.

**Code:**
```java
import java.util.Random;

class Solution {
    private int[] prefixSums;
    private Random rand;
    private int totalSum;

    public Solution(int[] w) {
        this.prefixSums = new int[w.length];
        this.rand = new Random();
        
        // Compute the prefix sum array
        int runningSum = 0;
        for (int i = 0; i < w.length; i++) {
            runningSum += w[i];
            prefixSums[i] = runningSum;
        }
        this.totalSum = runningSum;  // Total weight sum
    }
    
    public int pickIndex() {
        // Generate a random number between 0 (inclusive) and totalSum (exclusive)
        int target = rand.nextInt(totalSum);
        
        // Binary search to find the correct index
        int low = 0, high = prefixSums.length - 1;
        while (low < high) {
            int mid = low + (high - low) / 2;
            if (prefixSums[mid] > target) {
                high = mid;
            } else {
                low = mid + 1;
            }
        }
        return low;
    }
}
```

- **Time Complexity:** 
  - Preprocessing: O(N), where N is the length of the weights array to compute prefix sums.
  - Pick Index: O(log N) due to binary search.

- **Space Complexity:** 
  - O(N) for storing the prefix sum array.

---

### Binary Search Tree Approach (Optimal)

**Intuition:**

This approach uses a dynamic data structure like a balanced binary search tree (e.g., `TreeMap` in Java) to store cumulative weights, allowing for efficient cumulative lookup and random access. This is especially efficient if weights frequently update or if the number of weights (N) is very large.

1. Store cumulative weights in a `TreeMap`, where each key is a cumulative weight, and the value is the corresponding index.
2. For each `pickIndex` call, randomly determine a target and find the smallest key in the map greater or equal to the target.

**Code:**
```java
import java.util.Random;
import java.util.TreeMap;

class Solution {
    private TreeMap<Integer, Integer> map;
    private Random rand;
    private int totalSum;

    public Solution(int[] w) {
        this.map = new TreeMap<>();
        this.rand = new Random();
        
        int cumulativeSum = 0;
        for (int i = 0; i < w.length; i++) {
            cumulativeSum += w[i];
            map.put(cumulativeSum, i);
        }
        this.totalSum = cumulativeSum;
    }
    
    public int pickIndex() {
        int target = rand.nextInt(totalSum);
        // Find the smallest key greater than target. This gives the cumulative weight.
        return map.higherEntry(target).getValue();
    }
}
```

- **Time Complexity:**
  - Preprocessing: O(N log N) due to insertions into the tree map.
  - Pick Index: O(log N) since searching through the `TreeMap` is logarithmic on average.

- **Space Complexity:**
  - O(N) due to storing the cumulative sums in the `TreeMap`.

---

