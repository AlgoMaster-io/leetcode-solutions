# [Leetcode 354: Russian Doll Envelopes](https://leetcode.com/problems/russian-doll-envelopes/)

## Approaches
- [Approach 1: Dynamic Programming](#approach-1-dynamic-programming)
- [Approach 2: Binary Search with Sorting](#approach-2-binary-search-with-sorting)

---

## Approach 1: Dynamic Programming

### Intuition
The problem resembles finding the longest increasing subsequence, but here we must consider two dimensions: width and height. First, we can sort the envelopes based on width and use dynamic programming to find the longest increasing subsequence based on height.

### Steps
1. **Sorting:** Sort envelopes by increasing width. If two envelopes have the same width, sort by decreasing height (to avoid nesting envelopes with the same width).
2. **Dynamic Programming:** Use a DP array to record the longest increasing subsequence of heights after sorting by width.

### Code

```csharp
public int MaxEnvelopes(int[][] envelopes) {
    if (envelopes.Length == 0) return 0;
  
    // Sort by width asc, by height desc if widths are the same
    Array.Sort(envelopes, (a, b) => a[0] == b[0] ? b[1] - a[1] : a[0] - b[0]);
  
    int[] dp = new int[envelopes.Length];
    for (int i = 0; i < dp.Length; i++) dp[i] = 1; // Initialize dp array
  
    int maxEnvelopes = 1;
  
    for (int i = 1; i < envelopes.Length; i++) {
        for (int j = 0; j < i; j++) {
            // Check if envelope i can fit into envelope j
            if (envelopes[i][1] > envelopes[j][1]) {
                dp[i] = Math.Max(dp[i], dp[j] + 1);
            }
        }
        maxEnvelopes = Math.Max(maxEnvelopes, dp[i]);
    }
  
    return maxEnvelopes;
}
```

### Time Complexity
- Sorting: \(O(n \log n)\)
- Dynamic Programming: \(O(n^2)\)
Thus, the overall complexity is \(O(n^2)\).

### Space Complexity
- \(O(n)\) for the dp array.

---

## Approach 2: Binary Search with Sorting

### Intuition
The previous approach can be optimized using binary search. By maintaining a dynamic array and applying the concept of the longest increasing subsequence via binary search, we can achieve better time complexity.

### Steps
1. **Sorting:** Similar to the previous approach, sort envelopes by increasing width and decreasing height.
2. **Binary Search (LIS):** Use a list to store the current sequence of increasing heights. Use binary search (via `List<T>.BinarySearch`) to determine the position to replace or extend the subsequence.

### Code

```csharp
public int MaxEnvelopes(int[][] envelopes) {
    if (envelopes.Length == 0) return 0;

    // Sort by width asc, by height desc if widths are the same
    Array.Sort(envelopes, (a, b) => a[0] == b[0] ? b[1] - a[1] : a[0] - b[0]);

    List<int> dp = new List<int>();

    foreach (var envelope in envelopes) {
        int height = envelope[1];
        // Find the index to replace using binary search
        int index = dp.BinarySearch(height);
        if (index < 0) index = ~index;
        
        if (index == dp.Count) {
            // Extend the sequence
            dp.Add(height);
        } else {
            // Replace existing element with smaller height
            dp[index] = height;
        }
    }

    return dp.Count;
}
```

### Time Complexity
- Sorting: \(O(n \log n)\)
- Iteration + Binary Search: \(O(n \log n)\)
Thus, the overall complexity is \(O(n \log n)\).

### Space Complexity
- \(O(n)\) for the list storing the active sequence.

By optimizing with binary search, we achieve a significant speed-up for larger datasets compared to the DP approach.

