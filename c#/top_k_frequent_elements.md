# 347. [Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/)

## Approach 1: Min-Heap

### Solution
csharp
```csharp
// Time Complexity: O(n * log(k)), where n is the number of elements in nums
// Space Complexity: O(n), for the frequency map and heap
using System;
using System.Collections.Generic;
using System.Linq;

public class Solution {
    public int[] TopKFrequent(int[] nums, int k) {
        // Step 1: Build a frequency map
        Dictionary<int, int> frequencyMap = new Dictionary<int, int>();
        foreach (int num in nums) {
            if (frequencyMap.ContainsKey(num)) {
                frequencyMap[num]++;
            } else {
                frequencyMap[num] = 1;
            }
        }

        // Step 2: Use a min-heap to keep track of the top k elements
        PriorityQueue<int[], int> minHeap = new PriorityQueue<int[], int>();

        foreach (KeyValuePair<int, int> entry in frequencyMap) {
            minHeap.Enqueue(new int[] { entry.Key, entry.Value }, entry.Value);

            if (minHeap.Count > k) {
                minHeap.Dequeue(); // Remove the element with the smallest frequency
            }
        }

        // Step 3: Extract elements from the heap
        int[] result = new int[k];
        for (int i = 0; i < k; i++) {
            result[i] = minHeap.Dequeue()[0];
        }

        return result;
    }
}
```

## Approach 2: Bucket Sort

### Solution
csharp
```csharp
// Time Complexity: O(n), where n is the number of elements in nums
// Space Complexity: O(n), for the frequency map and buckets
using System;
using System.Collections.Generic;

public class Solution {
    public int[] TopKFrequent(int[] nums, int k) {
        // Step 1: Build a frequency map
        Dictionary<int, int> frequencyMap = new Dictionary<int, int>();
        foreach (int num in nums) {
            if (frequencyMap.ContainsKey(num)) {
                frequencyMap[num]++;
            } else {
                frequencyMap[num] = 1;
            }
        }

        // Step 2: Create buckets where index represents frequency
        List<int>[] buckets = new List<int>[nums.Length + 1];
        foreach (KeyValuePair<int, int> entry in frequencyMap) {
            int frequency = entry.Value;
            if (buckets[frequency] == null) {
                buckets[frequency] = new List<int>();
            }
            buckets[frequency].Add(entry.Key);
        }

        // Step 3: Collect the top k frequent elements
        int[] result = new int[k];
        int index = 0;
        for (int i = buckets.Length - 1; i >= 0 && index < k; i--) {
            if (buckets[i] != null) {
                foreach (int num in buckets[i]) {
                    result[index++] = num;
                    if (index == k) {
                        break;
                    }
                }
            }
        }

        return result;
    }
}
```

